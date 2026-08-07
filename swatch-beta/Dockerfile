# Local-build Dockerfile for the Home Assistant Supervisor add-on.
#
# The actual swatch application lives in a separate repo (this add-on repo
# only ever wrapped a prebuilt image from Docker Hub before), so the first
# stage below pulls that source in at build time instead. Change SWATCH_REPO
# via --build-arg if you're pointing this at your own fork.

FROM alpine/git:latest AS source
ARG SWATCH_REPO=https://github.com/MaiorDomus/swatch.git
ARG SWATCH_REF=main
# Home Assistant Supervisor passes --build-arg BUILD_VERSION=<config.yaml's
# version> on every build, including plain "Rebuild" with no other Dockerfile
# changes. Referencing it here (even though git doesn't need it) makes this
# RUN instruction's text differ whenever that version changes, which busts
# Docker's layer cache for this step specifically -- without it, this clone
# only ever runs once, on the very first build, and every later "rebuild"
# silently reuses that same stale checkout forever, no matter what's actually
# on the SWATCH_REF branch by then.
ARG BUILD_VERSION=dev
RUN echo "swatch source for add-on build ${BUILD_VERSION}" \
    && git clone --depth 1 --branch ${SWATCH_REF} ${SWATCH_REPO} /src

# web/pubspec.yaml pins `sdk: ">=2.17.0 <3.0.0"`, so this needs an old
# Flutter/Dart toolchain -- a modern one (Dart 3.x+) won't satisfy that
# constraint. Pulling the SDK directly from Google's archive rather than a
# third-party Docker image, since versions that old aren't reliably tagged
# on public registries.
FROM debian:bookworm-slim AS frontend-build
RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates xz-utils git \
    && rm -rf /var/lib/apt/lists/*
ARG FLUTTER_VERSION=3.3.10
RUN curl -fsSL "https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_${FLUTTER_VERSION}-stable.tar.xz" \
      -o /tmp/flutter.tar.xz \
    && tar -xf /tmp/flutter.tar.xz -C /opt \
    && rm /tmp/flutter.tar.xz \
    && git config --global --add safe.directory /opt/flutter
ENV PATH="/opt/flutter/bin:${PATH}"
COPY --from=source /src/web /web
WORKDIR /web
RUN flutter pub get && flutter build web

FROM crzynik/swatch-nginx:latest AS nginx

# Pinned to bookworm rather than the floating `python:3` tag: that tag has
# already drifted to Debian trixie, which dropped the libpcre3 package
# entirely (Debian's phasing out legacy PCRE1 in favor of PCRE2) -- but the
# crzynik/swatch-nginx binary above is linked against libpcre.so.3, so it
# needs a Debian release that still ships it. Pinning avoids this breaking
# again on a future rebuild if the tag drifts further.
FROM python:3-bookworm

COPY --from=source /src/requirements.txt /requirements.txt
COPY --from=source /src/requirements_dev.txt /requirements_dev.txt

ENV OPENCV_VERSION="4.5.1"

# ffmpeg is used to pull audio from camera RTSP streams for audio_monitors.
# libpcre3 provides libpcre.so.3, which the nginx binary above needs.
RUN apt-get update \
    && apt-get install -y --no-install-recommends ffmpeg libpcre3 \
    && rm -rf /var/lib/apt/lists/*

RUN pip install -r requirements.txt

COPY --from=nginx /usr/local/nginx/ /usr/local/nginx/

WORKDIR /opt/swatch/

COPY --from=source /src/swatch swatch/
COPY --from=frontend-build /web/build/web frontend/
COPY --from=source /src/migrations migrations/

COPY --from=source /src/docker/rootfs /

EXPOSE 4500

CMD ["python3", "-u", "-m", "swatch"]

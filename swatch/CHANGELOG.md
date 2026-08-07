# Versions

## 3.2.2-local

Pull in latest swatch changes (per-zone live detection status on the
dashboard). No Dockerfile changes -- this bump exists purely to trigger
a fresh git clone of the swatch repo per the 3.2.1-local cache-busting
fix.

## 3.2.1-local

Fix rebuilds silently reusing a stale checkout of the swatch repo forever --
the Dockerfile's `git clone` step had no cache-busting, so every "Rebuild"
after the very first install kept the exact source it cloned that first
time, no matter what actually changed upstream since. It's now tied to
Supervisor's own BUILD_VERSION build-arg, so bumping this version (which
you already need to do to get Supervisor to rebuild at all) also forces a
fresh clone.

## 3.2.0-local

Switch to a local build instead of pulling a prebuilt image -- this fork
isn't published to crzynik's Docker Hub, so Supervisor now builds the image
directly on install/update from a Dockerfile that pulls in the swatch repo
source and builds the frontend itself. Adds audio_monitors support (listens
to a camera's RTSP audio for sustained mechanical noise, e.g. a kitchen hood
fan) -- see the swatch repo's docs/config.md.

## 3.1.0

Adjust config so no dockerfile image is needed to be kept in backups

## 3.0.4

Fix crash where no object result is returned due to bad image

## 3.0.3

Fix crash when only one variant is named

## 3.0.2

Fix db path creation

## 3.0.1

Fix end time being updated for finished events

## 3.0.0

Objects are now tested based on bounding boxes that are created around clusters of pixels.

- Release Notes: https://github.com/NickM-27/swatch/releases/tag/3.0.0-beta1

## 2.2.3

Add more structured logs and fix db file not working correctly.

## 2.2.2

BREAKING: Move all swatch data to /media/swatch/... so it is all contained. 


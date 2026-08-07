# 2026-08 Dependency Upgrade Review

This add-on repo no longer wraps a prebuilt `crzynik/swatch` Docker image. Since this
fork doesn't have push access to that Docker Hub repo, both the `swatch` and
`swatch-beta` add-ons now build locally: Supervisor builds the image directly from each
add-on's `Dockerfile`, which clones the [`swatch`](https://github.com/MaiorDomus/swatch)
repo's source at build time and builds the frontend itself. There's no more
`push`/`push_beta` Makefile step, and no Docker Hub image to keep in sync.

The underlying `swatch` app was upgraded separately — see that repo's `UPGRADE.md` for
details (Flask 3.1, pydantic 2.13, peewee 4.3, numpy 2.5, opencv 5.0, etc.), and the
`swatch-hass-integration` repo's `UPGRADE.md` for the Home Assistant integration side
(now pinned to Home Assistant 2026.8.0).

## Picking up new swatch changes

Because the Dockerfile's `git clone` step is cached like any other Docker layer, a plain
"Rebuild" in Supervisor won't pick up new commits on its own. Bump the add-on's
`config.yaml` `version` field on every update — Supervisor passes that version as a
`BUILD_VERSION` build-arg on every build, which busts the clone step's cache and forces
a fresh checkout of the `swatch` repo's `main` branch.

## Related: snapshot retention default changed

The `swatch` app's default snapshot retention (`retain_days`) changed from 7 days to
1 day, so upgrading will start pruning `/media/swatch/snapshots` down to the last day's
worth of files automatically unless overridden per-camera in `swatch.yml`. See
`swatch`'s `docs/config.md` for the `retain_days` setting.

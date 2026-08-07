# 2026-08 Dependency Upgrade Review

This add-on repo just wraps the `crzynik/swatch` Docker image built from the
[`swatch`](https://github.com/MaiorDomus/swatch) repo — it has no Python/library
dependencies of its own (`config.yaml` only declares add-on metadata, ports, and volume
mappings), so there was nothing to bump here directly.

The underlying `swatch` image was upgraded separately — see that repo's `UPGRADE.md` for
details (Flask 3.1, pydantic 2.13, peewee 4.3, numpy 2.5, opencv 5.0, etc.), and the
`swatch-hass-integration` repo's `UPGRADE.md` for the Home Assistant integration side
(now pinned to Home Assistant 2026.8.0).

Once a new image is built and pushed from the `swatch` repo, this add-on's `config.yaml`
`version` field should be bumped to pick it up (per the existing release process in
`swatch/Makefile`'s `push`/`push_beta` targets).

## Related: snapshot retention default changed

The `swatch` app's default snapshot retention (`retain_days`) changed from 7 days to
1 day, so upgrading to the new image will start pruning `/media/swatch/snapshots` down to
the last day's worth of files automatically unless overridden per-camera in
`swatch.yml`. See `swatch`'s `docs/config.md` for the `retain_days` setting.

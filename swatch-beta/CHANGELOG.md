# Versions

## 3.2.14-local

New colorful filled-fan icon (coral/amber/teal) replacing the old
single-color teal outline -- this add-on's own icon.png is already
updated directly in this repo; this bump also pulls in the matching
favicon/PWA icon changes from the swatch repo. No Dockerfile changes --
this bump exists purely to trigger a fresh git clone of the swatch
repo per the 3.2.1-local cache-busting fix.

## 3.2.13-local

Pull in latest swatch changes (fix a crash when selecting text in the
highlighted config.yaml viewer). No Dockerfile changes -- this bump
exists purely to trigger a fresh git clone of the swatch repo per the
3.2.1-local cache-busting fix.

## 3.2.12-local

Pull in latest swatch changes (syntax highlighting for the Settings
config.yaml viewer). No Dockerfile changes -- this bump exists purely
to trigger a fresh git clone of the swatch repo per the 3.2.1-local
cache-busting fix.

## 3.2.11-local

Pull in latest swatch changes (camera cards now full-width, matching
the audio monitor/activity table; Settings page now actually works and
shows the raw config.yaml, read-only -- the sidebar link was previously
a dead end). No Dockerfile changes -- this bump exists purely to
trigger a fresh git clone of the swatch repo per the 3.2.1-local
cache-busting fix.

## 3.2.10-local

Pull in latest swatch changes (fix the dashboard being unusable behind
a reverse proxy that forces SSL -- API calls always used http://
regardless of the page's own scheme, which browsers block as mixed
content on an https page). No Dockerfile changes -- this bump exists
purely to trigger a fresh git clone of the swatch repo per the
3.2.1-local cache-busting fix.

## 3.2.9-local

Pull in latest swatch changes (dashboard layout: audio monitors and
the Recent Activity table now stack full-width below the camera grid
instead of sharing narrower grid cells; Recent Activity now shows 10
rows instead of 5). No Dockerfile changes -- this bump exists purely
to trigger a fresh git clone of the swatch repo per the 3.2.1-local
cache-busting fix.

## 3.2.8-local

Pull in latest swatch changes (dashboard "Recent Activity" table
showing the last 5 on/off transitions across all objects and audio
monitors; fixed detection history retention never actually deleting
anything -- now also prunes audio monitor history via a new
audio_monitors retain_days setting). No Dockerfile changes -- this
bump exists purely to trigger a fresh git clone of the swatch repo per
the 3.2.1-local cache-busting fix.

## 3.2.7-local

Fix two Supervisor deprecation warnings: `map`'s `config:rw` is
replaced with the `homeassistant_config` type (kept mounted at /config
via an explicit `path`, so CONFIG_FILE doesn't need to change), and
`armv7` is dropped from `arch` (removed by Supervisor).

## 3.2.6-local

Pull in latest swatch changes (object detection debounce now tracks
real elapsed time instead of counting auto_detect ticks -- fixes
longer-than-configured false-negative stretches; morphological closing
smooths noisy color-match blobs to raise solidity margin on small/noisy
objects). No Dockerfile changes -- this bump exists purely to trigger a
fresh git clone of the swatch repo per the 3.2.1-local cache-busting
fix.

## 3.2.5-local

Pull in latest swatch changes (dashboard auto-refresh with a
configurable interval dropdown -- Off/2s/5s/10s/30s/60s). No Dockerfile
changes -- this bump exists purely to trigger a fresh git clone of the
swatch repo per the 3.2.1-local cache-busting fix.

## 3.2.4-local

Pull in latest swatch changes (fix color_lower/color_upper being
matched against the wrong channels -- they were documented and produced
as R,G,B but matched against the image's native BGR order with no
reordering, so the first value gated Blue and the third gated Red;
color configs with a real R/B split need those two swapped to keep
matching the same color as before). No Dockerfile changes -- this bump
exists purely to trigger a fresh git clone of the swatch repo per the
3.2.1-local cache-busting fix.

## 3.2.3-local

Pull in latest swatch changes (debounced object/zone detection --
fixes flickering false negatives on small/noisy objects, new per-object
min_on_seconds/min_off_seconds config). No Dockerfile changes -- this
bump exists purely to trigger a fresh git clone of the swatch repo per
the 3.2.1-local cache-busting fix.

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


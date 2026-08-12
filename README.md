# gamegote.com

The public home for Gote Desert, a WebGPU sand game, and the place its progression is shown.

Static. No build step, no framework, no dependencies. `index.html` is the hub; `desert/` is the
project page plus every playable build ever shipped.

## Deploying

Vercel, as a static project, wired to `rotemgotlieb-dev/gamegote` through the **Git integration**:
a push to `main` is the production deploy. No build command, no output directory, no framework
preset. `gamegote.com` is attached under the project's Domains settings, with the DNS records at
GoDaddy.

The full weekly recipe, including the verification a release owes, is **`docs/RELEASE-RUNBOOK.md`**.
Read it before shipping a version.

## The builds

Copied from `sand-bending`, verified by checksum after the copy. **The cksum is the identity of a
build; the filename is not.**

| file | version | shipped | POSIX cksum |
|---|---|---|---|
| `desert/play/m0.html` | M0 foundation | 2026-07-29 | 57662288 |
| `desert/play/m1.html` | M1 terrain and sand material | 2026-07-31 | 1261274557 |
| `desert/play/m2.html` | M2 deformation, repose and carve | 2026-08-01 | 2240084894 |
| `desert/play/m3-pack.html` | **v1.0** character and generative score | 2026-08-11 | 334081415 |

Each date is the day that exact pack was minted in the `sand-bending` repo, read from its git
history. v1.0 is the pack tagged `video-2026-08-11-skeleton-ost`, the version Rotem ratified.

`desert/play/m3.html` is not a build. It is a **launcher**: v1.0 reads its feature flags from the URL
fragment (`#char=1&clips=1&audio=1`) and a shared link cannot be trusted to carry one, so the
launcher carries it instead. `desert/play/latest.html` is the stale-proof pointer at whatever the
current launcher is, which is what the hub's play button links to.

Versioning from here is **v1.1, v1.2** weekly. M0 to M2 keep the names they shipped under: the honest
build log is the point, and renaming a frozen artifact falsifies every citation to it.

## The page structure

`desert/index.html` renders itself from a single `RELEASES` array near the bottom of the file. That
array feeds both the PLAY THE LATEST slot and the progression rail, so adding a version is one entry
plus one file. Do not hand-edit the markup for a release.

## Requirements for a visitor

Desktop Chrome or Edge, on a keyboard. WebGPU is required, so phones and Safari are out for now. An
unsupported browser gets a single line of text, which is the build's own fatal path, not a broken
page.

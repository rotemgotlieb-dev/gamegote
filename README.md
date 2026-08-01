# gamegote.com

The public home for REPOSE, a WebGPU sand tech demo, and a place to show its progression.

Static. No build step, no framework, no dependencies. `index.html` plus three self-contained
game builds under `play/`.

## Deploying

Vercel, as a static project. No build command, no output directory, no framework preset.
Then add `gamegote.com` under the project's Domains settings and copy the DNS records it
prints into GoDaddy.

## The builds

Copied from `sand-bending`, verified by checksum after the copy:

| file | milestone | POSIX cksum |
|---|---|---|
| `play/m0.html` | M0 foundation | 57662288 |
| `play/m1.html` | M1 terrain and sand material | 1261274557 |
| `play/m2.html` | M2 deformation, repose and carve | 2240084894 |

When a new milestone packs, copy it in and re-verify the cksum against the source repo. The
number is the identity; the filename is not.

## Requirements for a visitor

Desktop Chrome or Edge. WebGPU is required and the controls are pointer and wheel based, so
phones and Safari are out for now. An unsupported browser gets a single line of text, which is
the build's own fatal path, not a broken page.

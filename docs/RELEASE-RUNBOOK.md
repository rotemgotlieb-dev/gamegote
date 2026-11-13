# RELEASE RUNBOOK: shipping v1.x to gamegote.com

Written 2026-08-11 with the v1.0 release. This is the mechanical recipe. Follow it exactly and a
weekly version takes a few minutes and cannot silently break an old link.

**The versioning scheme, in the owner's words:** the lead build is **v1.0**, and it goes **v1.1,
v1.2** weekly from here, "to make it feel more legit". The pre-v1 builds keep the names they
shipped under (M0, M1, M2). They are not renamed in hindsight: the honest build log IS the point,
and renaming a frozen artifact falsifies every citation to it.

## The law this runbook exists to protect

**Every URL that has ever worked keeps working, forever.** A rename or a new version never 404s an
old link. `/repose/*` still 308s to its `/desert/*` twin, and every `play/*.html` ever published
stays on disk exactly as it shipped. Adding a version is purely additive.

## What a release touches

| # | thing | why |
|---|---|---|
| 1 | `desert/play/<new>-pack.html` | the new build, copied byte-identical from the game repo |
| 2 | `desert/play/<new>.html` | the launcher that carries the URL fragment the build needs |
| 3 | one entry appended to `RELEASES` in `desert/index.html` | fills the PLAY slot and the progression rail |
| 4 | one line in `desert/play/latest.html` | the stale-proof pointer the hub links to |

Nothing else. The hub (`index.html`) is deliberately version-free so it never goes stale: it links
to `latest.html`, not to a version number.

## Naming

v1.0 shipped as `m3.html` / `m3-pack.html` because it is milestone 3 of the game project and those
URLs are now public. **From v1.1 onward use the version in the filename**: `v1.1.html` and
`v1.1-pack.html`. The page never hardcodes a filename, it reads `href` out of the ledger, so the two
conventions coexist without a special case.

## The recipe

### 1. Copy the pack, byte-identical, and prove it

The cksum is the identity of a build. The filename is not.

    cksum ~/conductor/repos/sand-bending/dist/index.html
    cp ~/conductor/repos/sand-bending/dist/index.html \
       ~/conductor/repos/gamegote/desert/play/v1.1-pack.html
    cksum ~/conductor/repos/gamegote/desert/play/v1.1-pack.html
    cmp ~/conductor/repos/sand-bending/dist/index.html \
        ~/conductor/repos/gamegote/desert/play/v1.1-pack.html && echo IDENTICAL

The two cksums must match and `cmp` must print IDENTICAL. If the game repo tagged the build (it
should: v1.0 came from tag `video-2026-08-11-skeleton-ost`), record the tag in the commit message.

### 2. Write the launcher

A pack reads its feature flags from the URL fragment, and a shared or bookmarked link cannot be
trusted to carry one. The launcher is what makes a bare link boot the right experience.

Copy `desert/play/m3.html`, change the target, and change nothing else. The target is written three
times: the `TARGET` line at the top, the `<noscript>` refresh, and the Continue link. The copy keeps
the launcher's WebGPU check, so a browser that cannot run the build is told so instead of going
black. The v1.0 target is:

    m3-pack.html#char=1&clips=1&audio=1

`char=1` is the figure, `clips=1` its hybrid idle, `audio=1` the generative score. **Do not add a
skin flag**: the wireframe skeleton is the ratified look, not a fallback to be fixed.

Confirm the flag names against the game repo's own parser before trusting them:

    grep -REo "(^|[#&])[a-z]+=1" ~/conductor/repos/sand-bending/src/

### 3. Append one entry to the release ledger

In `desert/index.html`, at the END of the `RELEASES` array (the array is oldest first, and the last
entry is what fills the PLAY slot and lights the last node of the rail):

    {
      id: "v1.1",
      name: "<two or three plain words>",
      date: "2026-08-18",
      href: "play/v1.1.html",
      cksum: 123456789,
      note: "<one sentence, in plain language, about what a player can now do>"
    }

Three rules for that entry, all of them house law:

- **`date` is the day the pack was minted, read from the game repo's git history**, never guessed:
  `git log -1 --format=%ad --date=short -- <pack file>`.
- **`note` is derived, never invented.** Take it from the build's own README section, its DECISIONS
  entry, or its tag message. A fact about the world beats a fact about the code: "someone walks the
  desert" beats "added a locomotion module".
- **`cksum` is the number you printed in step 1.** It is the record that this page links to that
  exact build.

### 4. Move the pointer

In `desert/play/latest.html`, change the single `location.replace(...)` target and the matching
`<meta http-equiv="refresh">` and `<a href>` to the new launcher. Three occurrences of one filename,
all in the first fifteen lines.

### 5. Update the honest ledger, because a stale one is worse than none

`desert/index.html` carries a "Working now" and a "Not built yet" list. If the new build moved
something across that line, move it. This section only earns its place by staying true.

### 6. Verify locally BEFORE deploying

Serve the repo and drive a real browser. Pick a port nothing else is using: other sessions run
servers on this machine and a busy port silently serves you someone else's site.

    cd ~/conductor/repos/gamegote
    PORT=8934; while lsof -nP -iTCP:$PORT -sTCP:LISTEN >/dev/null 2>&1; do PORT=$((PORT+1)); done
    python3 -m http.server $PORT --bind 127.0.0.1

Then confirm, in a browser:

- `/desert/` renders, the PLAY slot shows the NEW version, and the rail ends on it.
- The launcher chain lands on the pack WITH the fragment: open `/desert/play/latest.html` and read
  the final URL. It must end in the full flag string.
- Every older link still opens its own build.
- No horizontal scroll at 390 px wide (`document.documentElement.scrollWidth` equals `clientWidth`).

### 7. Deploy

**gamegote.com is served by Vercel from `rotemgotlieb-dev/gamegote` through the Git integration.**
A push to `main` IS the production deploy: the live production deployment carries the
`gamegote-git-main-...` alias, which only the Git integration creates. There is no CI config in this
repo and no build step.

    git add <the exact paths you touched>     # never -A, other sessions commit here
    git commit
    git push origin main

Watch it land:

    vercel ls gamegote        # the new Production row appears within a minute or two

The CLI (`vercel --prod`) is the fallback if the Git integration ever stops firing. Prefer the push:
it keeps the deployed bytes and the committed bytes provably the same thing.

**A deploy is a publish, so it is Rotem's call, not an agent's**, unless he has said to ship.

### 8. Verify the LIVE site, not the local one

Run this without a pipe so the exit status is the check's own:

    for u in / /desert/ /desert/play/m0.html /desert/play/m1.html /desert/play/m2.html \
             /desert/play/m3.html /desert/play/m3-pack.html /desert/play/latest.html \
             /desert/play/v1.1.html /desert/play/v1.1-pack.html; do
      curl -sS -o /dev/null -w "%{http_code}  $u\n" "https://gamegote.com$u"
    done

Every one must be 200. Then the legacy paths, which must 3xx to their MATCHING `/desert/` twin and
not to the landing page:

    for p in / /play/m0.html /play/m1.html /play/m2.html; do
      curl -sS -o /dev/null -w "%{http_code} %{redirect_url}\n" "https://gamegote.com/repose$p"
    done

And the SLASHLESS forms, which must 3xx to the slashed page and never serve it directly (the
2026-08-11 Fable review found bare `/desert` serving the index with every relative link broken,
while the battery only ever tested the slashed form):

    for b in /desert /desert/play /repose; do
      curl -sS -o /dev/null -w "%{http_code} %{redirect_url}  $b\n" "https://gamegote.com$b"
    done

Then prove the deployed bytes are the bytes you checksummed:

    curl -sS -o /tmp/live-pack.html https://gamegote.com/desert/play/v1.1-pack.html
    cksum /tmp/live-pack.html      # must equal step 1's number

A matching content-length is weaker evidence than a matching cksum. Take the cksum.

## The two failure modes this runbook is built against

1. **A deep link degrading to a landing page.** Redirects are enumerated per file in `vercel.json`
   with the catch-all LAST, so `/repose/play/m1.html` lands on M1 and not on the hub. A redirect
   that lands somewhere plausible looks like it worked, which is why the check above prints
   `redirect_url` and not just the status code. Full reasoning: `docs/REDIRECTS.md`.
2. **A "new version" that is not the build anyone ratified.** Only the cksum settles that. Copy,
   checksum, and record the number in the ledger and the commit message.

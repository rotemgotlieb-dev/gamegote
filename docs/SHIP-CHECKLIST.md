# SHIP CHECKLIST: publishing v1.1 to gamegote.com

Staged 2026-08-13 on branch `v1.1-prep`. This is the owner's read when he says go. It is the
runbook's recipe (`docs/RELEASE-RUNBOOK.md`) with this release's specifics already filled in, plus
the three gates that are his and not a script's.

**Nothing on this branch is live.** The branch changes the page's text and stages a release entry
that is commented out. It contains no pack, no launcher, and no deploy.

**Read this top to bottom once before starting.** Steps 3 through 7 are one commit. Publishing half
of them is how a page ends up advertising controls that the live build does not have.

---

## THE THREE GATES THAT ARE YOURS

Nothing below step 0 happens until all three are settled. Two of them are taste and one is a number.

### GATE 1: your taste pass on the build

Play the composed build and settle the calls that are yours. They are all still open:

| open call | what ships if you say nothing |
|---|---|
| **Which camera opens the game** | free cam, because it is the one you said you like and every gate is calibrated against it |
| **Five modes on C, or split across C and V** | all five on C, one ring |
| **The glide entry default** | the in-air re-press, with the old hold one click away in F1 > Character |
| **The figure's colour: dark ink, bone white, or the ink halo** | dark ink (4.1 contrast in sun, 2.2 in shade, which is the shade complaint you raised) |
| **The release name** | "Cameras and the map". Alternates in `docs/RELEASE-NOTES-v1.1-DRAFT.md` |
| **The release notes themselves** | the draft, verbatim, in that same file |

The build to play is the composed pack already on your desktop:
`~/Desktop/GoteDesert-videos/2026-08-13-composed-1221556362.html`, which is what
`PLAY-GOTE-DESERT.html` points at.

### GATE 2: the pacing gate, green, on the pack that actually ships

This is the one number that blocks the release, and it is not green today.

- W10-27 measured glide pacing hitches climbing across merge boundaries: **1** on the live v1.0 pack,
  **17 to 24** on the current gated line, A/B interleaved so the delta sits on the pack and not on
  the machine.
- W10-28 item 6 found the existing floor is **report-only**: two committed known-good runs read
  41.5 to 57.8 fps and then 28.2 to 31.4 fps while the verdict stayed PASS. A 2x regression landed
  green.
- The gate that would catch it (`dashsmooth`'s glide drive with a hitch-count budget, armed by a
  planted stall) is **proposed, not built**.

Shipping before this is green publishes your own "jumps around" complaint to strangers.

### GATE 3: the next merge window has composed the pack

The pack these notes were written against is the MERGE WINDOW 4 composition, `1221556362`. It is the
build you are playing, not the build that ships. The v1.1 pack is whatever the next merge window
composes, and **its cksum is the only number that may enter the release ledger.**

---

## THE STEPS

### 0. Take the branch and confirm it is still the branch you reviewed

```
cd ~/conductor/repos/gamegote
git checkout v1.1-prep
git pull --ff-only origin v1.1-prep
git log --oneline -3
```

### 1. Re-verify the Controls block against the pack that is SHIPPING

The controls list was written against `1221556362`. If the shipping pack is a later composition,
re-check each row rather than trusting this one. Run in the game repo, against the pack you are about
to copy:

```
cd ~/conductor/repos/sand-bending
P=dist/index.html
for s in KeyE KeyC "THE PLAINS" "THE MOUNTAINS" BracketRight glideEntry ghostHalo Collapse "build or dig" "Free cam" "First person"; do
  printf "%-16s %s\n" "$s" "$(grep -c -- "$s" $P)"
done
```

Every count must be at least 1. A zero means the page would advertise a control the build does not
have: fix the page, not the count.

### 2. Copy the pack, byte-identical, and prove it

```
cd ~/conductor/repos/sand-bending
cksum dist/index.html
cp dist/index.html ~/conductor/repos/gamegote/desert/play/v1.1-pack.html
cksum ~/conductor/repos/gamegote/desert/play/v1.1-pack.html
cmp dist/index.html ~/conductor/repos/gamegote/desert/play/v1.1-pack.html && echo IDENTICAL
```

Write the number down. It goes in three places: the ledger entry, the commit message, and the live
check at the end.

### 3. Write the launcher

The launcher exists so a bare or shared link boots with the right flags. Copy the v1.0 one and change
only the target:

```
cd ~/conductor/repos/gamegote/desert/play
cp m3.html v1.1.html
```

Then in `v1.1.html` change all three occurrences of `m3-pack.html#char=1&clips=1&audio=1` to
`v1.1-pack.html#char=1&clips=1&audio=1` (the `location.replace`, the `meta refresh`, and the
`Continue` link), and change the title and body text from v1.0 to v1.1.

The three flag names were re-confirmed against the game repo's own parsers this session:
`char=1` (`src/character/character.js:442`), `clips=1` (`src/character/skin.js:68`),
`audio=1` (`src/audio/context.js:52`). **Do not add a skin flag.**

### 4. Turn on the prepared release entry

In `desert/index.html`, find the block that starts `v1.1, PREPARED AND DELIBERATELY NOT LIVE`.

1. Delete the comment's opening `/* ===...` line and its closing `=== */` line, keeping the entry
   itself and the leading comma.
2. Replace `cksum: 0` with the number from step 2.
3. Replace `date: "PLACEHOLDER-NOT-A-FACT"` with the day the pack was minted, read from git, never
   guessed:
   ```
   cd ~/conductor/repos/sand-bending && git log -1 --format=%ad --date=short -- dist/index.html
   ```
4. Replace `name` with your pick from gate 1.
5. Replace `note` with the ledger line from `docs/RELEASE-NOTES-v1.1-DRAFT.md`, or your edit of it.

### 5. Turn on the no-JavaScript twin

In the same file, in the `<noscript>` block: uncomment the prepared `v1.1` line and delete
`(latest)` from the v1.0 line below it. Both edits, or a reader with scripting off sees the wrong
build called latest.

### 6. Move the one pointer

In `desert/play/latest.html`, change `m3.html` to `v1.1.html` in all three places (the
`location.replace`, the `meta refresh`, and the `Continue` link) and update the
`Current target: v1.0` comment. All within the first fifteen lines.

### 7. Verify locally, before anything is pushed

```
cd ~/conductor/repos/gamegote
PORT=8934; while lsof -nP -iTCP:$PORT -sTCP:LISTEN >/dev/null 2>&1; do PORT=$((PORT+1)); done
echo "http://127.0.0.1:$PORT/desert/"
python3 -m http.server $PORT --bind 127.0.0.1
```

In a browser, confirm all five:

- `/desert/` renders, the PLAY slot reads **v1.1**, and the rail ends on it, five nodes.
- `/desert/play/latest.html` lands on the pack **with the full fragment** in the final URL.
- Every older link still opens its own build: m0, m1, m2, m3.
- The Controls list matches what the build actually does, key by key, with the build open beside it.
- At 390 px wide, `document.documentElement.scrollWidth === clientWidth`.

### 8. Commit, on explicit paths only

Other sessions commit in this repo. Never `git add -A`.

```
cd ~/conductor/repos/gamegote
git add desert/index.html desert/play/v1.1.html desert/play/v1.1-pack.html \
        desert/play/latest.html docs/RELEASE-NOTES-v1.1-DRAFT.md docs/SHIP-CHECKLIST.md
git commit    # put the pack cksum and the game-repo commit or tag in the message
```

### 9. Publish. This click is yours.

A push to `main` **is** the production deploy. There is no build step and no CI in this repo.

```
git checkout main && git pull --ff-only origin main
git merge --no-ff v1.1-prep
git push origin main
vercel ls gamegote     # the new Production row appears within a minute or two
```

### 10. Verify the LIVE site, not the local one

Run it without a pipe so the exit status is the check's own. Every line must read 200:

```
for u in / /desert/ /desert/play/m0.html /desert/play/m1.html /desert/play/m2.html \
         /desert/play/m3.html /desert/play/m3-pack.html /desert/play/latest.html \
         /desert/play/v1.1.html /desert/play/v1.1-pack.html; do
  curl -sS -o /dev/null -w "%{http_code}  $u\n" "https://gamegote.com$u"
done
```

The legacy paths must 3xx to their matching `/desert/` twin, not to the landing page:

```
for p in / /play/m0.html /play/m1.html /play/m2.html; do
  curl -sS -o /dev/null -w "%{http_code} %{redirect_url}\n" "https://gamegote.com/repose$p"
done
for b in /desert /desert/play /repose; do
  curl -sS -o /dev/null -w "%{http_code} %{redirect_url}  $b\n" "https://gamegote.com$b"
done
```

Then prove the deployed bytes are the bytes you checksummed:

```
curl -sS -o /tmp/live-pack.html https://gamegote.com/desert/play/v1.1-pack.html
cksum /tmp/live-pack.html      # must equal the number from step 2
```

A matching content length is weaker evidence than a matching cksum. Take the cksum.

---

## WHAT THIS BRANCH ALREADY DID, so you are not re-checking it

- The Controls block is rewritten and every row was read off pack `1221556362`, with the file and
  line for each row recorded in an HTML comment above the block.
- The "Working now" and "Not built yet" ledger is updated for the same pack, including the honest
  line that terraforming does not follow you away from the spawn area.
- The v1.1 release entry, the no-JavaScript twin line, and the player-facing notes are written and
  staged, all inert.
- The page was rendered headless at 390 px from this branch: 0 page errors, no horizontal scroll,
  4 rail entries with v1.0 still last, which is the proof the prepared entry is genuinely inert.

## WHAT THIS BRANCH DELIBERATELY DID NOT DO

- No pack copied, no launcher written. Both need the cksum that does not exist yet, and a launcher
  pointing at a missing pack is exactly the 404 the runbook's first law exists to prevent.
- No `latest.html` change. It points at v1.0 and v1.0 is what is live.
- Nothing merged to `main`, nothing deployed, no `vercel` command run.
- `M` was not written into the map row. The owner's call is to bind both M and E (A-34), but in
  `1221556362` M is still only the mute key, so the M row sits ready as a comment beside the E row
  and lands when the a202 lane binds it.

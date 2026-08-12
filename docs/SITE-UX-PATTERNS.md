# SITE-UX-PATTERNS.md: how game sites present playable builds like a game

Researched 2026-08-11, Rotem's commission verbatim: "search up how we can progressively give it a
gamified feel... how a site should present builds and levels in a fun and gamified way." Scope is
the TWO-PAGE site as it exists today: `gamegote.com` hub (`index.html`, one live card: Gote Desert)
and `gamegote.com/desert/` (`desert/index.html`, a 3-card grid: Milestone 0 Foundation, Milestone 1
Terrain and light, Milestone 2 Deformation and carve, current-badge on M2). Prior scoping already
exists at `docs/SITE-PLAN.md` (queued, unbuilt): read that first, it decomposes Rotem's ask into
"newest build in one click," "gamified feel," "More About This Project" deep dive, and names where
the deep-dive material already lives (DECISIONS.md, PERF.md, EARLYOUT-LADDER.md, the harness
stills/clips). This file is narrower: not what to show, but the PRESENTATION MECHANISM that makes a
version list read as a game rather than a changelog.

**Hard boundary, per the flagship-visual law (vault CLAUDE.md, no-freehand-in-code rule):** every
pattern below is typography, layout, motion, and microcopy only. None require new illustration,
iconography, mascot art, or a logo mark. Where a pattern's usual form leans on custom art (pixel-art
badges, a hand-drawn cartridge), the adoption note says so and gives the type/motion-only substitute.
Flagged OUT explicitly where relevant.

---

## 1. Level select as version history (the framing Rotem asked to find)

**What it is.** Instead of a changelog or a flat "v0.1 / v0.2 / v0.3" list, milestones are presented
as a row of LEVELS: numbered, left-to-right or in a grid, each a discrete node you enter. Mario-style
world maps and roguelike stage-select screens are the canonical form (Game UI Database's dedicated
"Level Select: World Map" and "Stage/Level Select" pattern pages catalog dozens of shipped
examples) [Game UI Database](https://www.gameuidatabase.com/index.php?scrn=6).

**Why it reads as game-like.** A changelog is read passively (prose, scanned top to bottom). A level
select is ENTERED: each node is a destination with a state (locked / current / cleared), and picking
one is itself a small decision with a payoff. The mechanism is agency: version history becomes
something you navigate rather than something you're told.

**Adoption note.** `desert/index.html` already has 90% of this: a card grid, monospace "Milestone N"
eyebrows, and a `.current` badge ("Start here") on M2. What is missing is the LEVEL-STATE language:
right now every card reads identically except the current one. Add a second visual state, CLEARED
(the milestones behind the current one, meaning "fully superseded, safe to skip unless you want the
history"), done with a text tag and a dimmed border only, e.g. `<span class="badge cleared">Earlier
build</span>` at 70% opacity, no new art. When milestone 3 (character, movement, terraforming,
generative OST) ships, it slots in as a fourth node using the exact same card component: the pattern
scales without a redesign, which is the point of picking it now.

---

## 2. The "currently playable" signal (distinguishing a demo reel from a live build)

**What it is.** A small, unmistakable marker that says "this one runs, right now, in your browser,"
distinct from screenshots or video. Itch.io's own convention is the download/play button plus a
platform icon row directly under the game title; the moment a page shows that row, visitors read it
as launchable, not archival [itch.io: Designing your page](https://itch.io/docs/creators/design).

**Why it reads as game-like.** Sites default to "look at this" (screenshots, prose). A playable
signal flips the default to "do this," which is the entire difference between a portfolio piece and
a game. The mechanism is a promise made explicit in one glance: no scrolling required to learn
whether clicking does something.

**Adoption note.** The hub already has this partially: `.meta` reads "WebGPU &middot; in
development" on the Gote Desert card. Sharpen it: the eyebrow line on the CURRENT milestone card
should say `PLAYABLE NOW` (monospace, accent color, already-defined CSS token `--sun`/`--accent`),
and every other milestone's eyebrow says `PLAYABLE · earlier build` in the muted ink-3 tone. No new
art, just a copy and color-weight change using tokens that already exist in both files' `:root`.

---

## 3. The pre-flight / "before you click" gate as a boot ritual

**What it is.** A short, unskippable block of text or screen that runs before the game itself:
requirements, controls, or a content note, styled distinctly from the rest of the page. Classic
console boot splashes and "Press Start" screens serve the same function physically: a beat between
"I arrived at this page" and "I am now inside the game" [Wikipedia: Start
screen](https://en.wikipedia.org/wiki/Start_screen); modern research on loading screens finds this
pause functions as a mood-setter and a signal of production quality, not just a technical wait
[Medium: Loading Screens and Liminal
Spaces](https://medium.com/@franziska.fink.rpllc/loading-screens-and-liminal-spaces-why-we-miss-waiting-ab102aa328a1).
Adobe's 2026 trend note on the "Press Start" aesthetic makes the same point from the design-trend
side: the ritual itself, not the graphics, is what reads as game-native [Adobe: Press
Start](https://www.adobe.com/express/learn/blog/2025-microtrends-press-start).

**Why it reads as game-like.** Web pages don't normally gate you before content; games do, constantly
(splash screens, controller-check prompts, "any button to continue"). A deliberate beat before entry
borrows that grammar and signals "what's past this point is a system, not a page."

**Adoption note.** `desert/index.html` already has the ingredients: the `.req` box ("Before you
click: Desktop Chrome or Edge...") sits above the milestone grid. Reframe it AS the boot gate rather
than a caveat: move it to sit directly above the level-select grid with a `press ▸ to continue`-style
microcopy treatment (monospace, the existing `kbd` style already defined for the controls list) and
let the grid itself feel like "what's past the gate." Zero new art; this is a reorder plus a type
treatment using the file's own existing `.req` and `kbd` classes.

---

## 4. Live devlog feed as a session log, not a blog

**What it is.** Itch.io's devlog system attaches a running, timestamped post feed directly to the
game page, distinct from the store description, and surfaces the most recent entries automatically
[itch.io: Introducing devlogs](https://itch.io/updates/introducing-devlogs). The framing that makes
it read as game-native rather than corporate-blog is voice: entries are written in first person, in
the moment ("today I fixed X," "here's a weird bug I found"), not as press releases.

**Why it reads as game-like.** It mirrors how players themselves narrate playthroughs (a session log,
a raid report, a run recap). A dev writing in the same register as a player narrating their session
collapses the distance between "the team" and "the community," which is exactly Rotem's "learnings
of the project, important decisions... things we're proud of" ask from SITE-PLAN.md.

**Adoption note.** This is the mechanism for the "More About This Project" deep dive SITE-PLAN.md
already scoped. Curate DECISIONS.md entries (already written in first-person, present-tense,
narrated voice: "the owner asked for X, here's what shipped") as-is into dated devlog cards rather
than rewriting them into marketing copy. Layout only: a stacked card list, newest first, monospace
date eyebrows matching the milestone cards' visual language. No new art.

---

## 5. Screenshot/clip strip as proof-of-play, not hero imagery

**What it is.** A horizontal strip or grid of small, real, in-context captures (not a single polished
hero shot) placed close to the play button, standing in for "here is what actually happens when you
play," sourced directly from the build rather than staged.

**Why it reads as game-like.** Marketing hero images promise a mood; a strip of real captures proves
a mechanic. The mechanism is evidentiary: multiple small, unglamorous frames read as "recorded from
the thing itself," which is more convincing for a WebGPU tech demo than one beautiful still.

**Adoption note.** `sand-bending/harness/results/**/stills/` already has these, generated by the
harness at delivered size (per memory `platform-affordances-are-verified-not-assumed` and
`verify-the-delivered-output-not-the-attribute`: use the actual harness output, not a fresh screenshot
taken by hand). Lay out as a CSS grid of small frames under each milestone card in the deep-dive
view, captioned with the plain-English fact they demonstrate ("avalanche settling to repose angle,"
not "screenshot_04.png"). Zero new art: these are already-generated, non-flagship, documentary
captures, not brand imagery, so the flagship-visual gate does not apply to them.

---

## 6. The honest "working / not built yet" ledger as a difficulty-style transparency device

**What it is.** A visible, two-column (or two-list) split of what's real right now versus what's
coming, stated plainly rather than hidden in a roadmap page.

**Why it reads as game-like.** Early-access and playtest builds on Steam and itch.io routinely carry
this exact device ("known issues," "not yet implemented") and players read it as a sign of a live,
evolving system rather than a finished, static product; it sets expectations the way a difficulty
label or a beta tag does, without needing either.

**Adoption note.** Already built and shipping: `desert/index.html`'s `.honest` block ("Working now" /
"Not built yet"). Nothing to add here except keeping it current as milestones ship (the bending verb,
wind, and post-processing lines need to move from "not built" to "working" as those land, and the
character/movement/OST work from the CONTEXT needs its own working-now line the next time this page
is touched). Flag: this pattern's value depends entirely on staying truthful and current; a stale
ledger is worse than none, so it is a maintenance item, not a one-time build.

---

## 7. Controls legend styled as an input-method card, not a help page

**What it is.** A compact, glanceable list of verbs mapped to inputs, styled like an arcade cabinet's
control-panel decal or a fighting-game move list, rather than a prose "how to play" paragraph.

**Why it reads as game-like.** Verb-to-input tables are a genre convention players already pattern-
match instantly (fighting game move lists, platformer control diagrams); a prose paragraph saying
"click and drag to carve" reads as documentation, the identical fact in a `kbd`-styled table reads as
a game's control scheme.

**Adoption note.** Already built: `desert/index.html`'s `.controls` list with styled `<kbd>` tags.
Keep this component and extend it verbatim for milestone 3 once walk/run/jump/glide/slide and
terraforming verbs are live; it is already the right shape and needs no redesign, only more rows.

---

## 8. Recommended-entry badge instead of chronological default

**What it is.** Rather than assuming a visitor starts at v1 and reads forward, the newest/best build
is marked as the entry point explicitly ("Start here"), while earlier builds stay reachable but
de-emphasized.

**Why it reads as game-like.** This mirrors how games handle "continue" vs "new game" on a title
screen: the system has an opinion about where you should land, stated plainly, rather than making the
visitor infer it from a list order. It directly answers Rotem's ask ("newest version should be at the
top in one click").

**Adoption note.** Already built: the `.current` class and `Start here` badge on M2 in
`desert/index.html`. This is the pattern to keep reusing verbatim as new milestones ship: move the
badge, don't reinvent it. The hub-level version of this pattern (SITE-PLAN.md's ask #1, "play the
newest build in ONE CLICK, at the top") is not yet built on `index.html`: the `Gote Desert` card
links to `/desert/` (the hub, not the current build directly). Concrete fix: make the hub card's
primary link point straight at the current milestone (`/desert/play/m2.html`, or whatever is current
at ship time) with a secondary, smaller link to "see all builds" for `/desert/`. One-click-to-play
from the hub, per the ask, with zero new art.

---

## 9. Sound-on prompt as a diegetic boot beat (this game already has the hook for it)

**What it is.** Where audio exists but can't autoplay (every modern browser blocks audio before a
user gesture), sites turn the unlock itself into a small ritual moment rather than a silent gate: a
speaker icon, a "click to enable sound" microcopy line, sometimes styled as a physical dial or
switch.

**Why it reads as game-like.** It borrows the console-boot beat (pattern 3) but ties it to something
mechanically true about the browser rather than being decorative, so it reads as "the system is real"
rather than "the system is theater."

**Adoption note.** This is grounded directly in the repo: `docs/DECISIONS.md` W10-15 confirms the
generative OST is real and already built, gated behind a real user gesture by construction
(`context.js`, "created only inside the first real user gesture") and currently shipped OFF by
default behind `#audio=1`. When audio ships on by default, the site's play-button microcopy should
say so plainly ("Sound on. Click to start.") rather than silently launching muted, which is both more
honest and reads as more game-like than a silent boot. Type and microcopy only, no new art; do not
build this until the audio flag actually flips on by default, since claiming a sound-on game before
it is one would violate the confirmed-is-not-happened rule.

---

## 10. Milestone numbering as a build log, kept honestly incomplete rather than padded

**What it is.** Using "Milestone N" / "M0, M1, M2" labels instead of marketing version names (no
"Alpha," "Genesis Update"), and NOT padding the count with filler entries to look further along than
the project is.

**Why it reads as game-like.** Paradoxically, under-selling reads as more credible to a technical or
enthusiast audience than a bloated roadmap; itch.io devlog culture rewards exactly this (the visible
"final devlog" and dated version-number posts researched above are matter-of-fact, not hyped) and it
matches Rotem's own stated house style (matter-of-fact tone, no filler, per vault CLAUDE.md).

**Adoption note.** Already the site's convention (`Milestone 0/1/2`, plain names: "Foundation,"
"Terrain and light," "Deformation and carve"). Keep it exactly as-is when milestone 3 ships; resist
the urge to rename past milestones into something punchier in hindsight, since the honest, working
build log IS the gamified hook here, not a layer added on top of it.

---

## Top 3 for a first pass (ranked by effort-to-signal ratio, all buildable now, zero new art)

1. **Pattern 8, the hub-to-current-build one-click link.** Directly closes SITE-PLAN.md's ask #1 and
   is a one-line `href` change plus a secondary link.
2. **Pattern 1, level-state language on the milestone cards** (`PLAYABLE NOW` / cleared / earlier
   build). Small CSS + copy change on an existing component, immediately makes the version list read
   as a level select rather than a changelog.
3. **Pattern 3, the boot-gate reframe** of the existing `.req` box. Pure reorder + type treatment of
   content already on the page.

---

---

# APPLIED, 2026-08-11 night (the v1.0 build sprint)

Written by the builder lane that consumed this file about fifteen minutes after it landed. This
section is the other half of the research contract: what was actually taken, what was changed on
contact with the build, and what was left. The file above is unedited.

## Taken as written

| # | pattern | where it landed |
|---|---|---|
| 8 | recommended entry, and one click to play from the hub | `index.html`: the Gote Desert card's primary action is now `PRESS PLAY`, with `Every build & the details` demoted to a secondary ghost button. `desert/index.html` leads with a PLAY THE LATEST slot above everything except the boot gate. |
| 1 | level select as version history, with level STATE language | the progression rail in `desert/index.html`. Every node carries a state: `CURRENT BUILD` (lit dot, sun-colored id) or `EARLIER BUILD` (dimmed to 78 percent, brightening on hover). |
| 2 | the currently-playable signal | a `PLAYABLE NOW` pill in the live green on both pages, next to the version number. |
| 3 | the boot gate | the requirements block was reframed from a caveat into a beat you pass through: monospace, sun-colored left rule, headed `BEFORE YOU PRESS PLAY`, and positioned directly above the play slot. |
| 7 | controls as an input-method card | kept verbatim and extended with v1.0's verbs. It needed more rows and no redesign, exactly as predicted. |
| 6 | the honest working / not-built ledger | kept, and moved on contact with reality: the character, the movement verbs and the generative score crossed from "not built" into "working now", and two new honest lines were added, including that the figure's LOOK is deliberately unbuilt. |
| 10 | honest milestone numbering, no renaming in hindsight | M0, M1 and M2 keep the names they shipped under, including inside the frozen packs. |

## Changed on contact

**Pattern 9 (sound-on as a boot beat) shipped, and its stated precondition is now met.** The pattern
says not to build it "until the audio flag actually flips on by default, since claiming a sound-on
game before it is one would violate the confirmed-is-not-happened rule". That was the right call
against the pack alone, where `#audio=1` is off by default. The launcher changed the fact: every
public route into v1.0 goes through `desert/play/m3.html`, which boots the pack with
`#char=1&clips=1&audio=1`. Audio is therefore on by default **for every visitor**, verified by
driving the real chain in a browser and reading the flag back off `location.hash`. The microcopy
shipped is "Sound is on. It starts at your first click, because a browser will not let a page make
noise before you touch it," which is both the honest statement and the pattern's diegetic beat.
**The general lesson: a precondition can be satisfied by the route rather than by the build.**

**Pattern 10 met the owner's versioning ask, and both survived.** He asked for v1.0, then v1.1 and
v1.2 weekly, "to make it feel more legit". Pattern 10 warns against marketing version names and
against renaming past milestones. They are compatible: v1.0 is the honest name for the first build
that is a game rather than a tech demo, and M0 to M2 stay exactly as they shipped, as the pre-v1 era
of the rail. Nothing was renamed backwards and no filler entry was invented to pad the count.

**Pattern 1's card grid became a vertical rail.** The owner's added ask was a PROGRESSION, "show the
progression of the game throughout time", which is a different job from a level-select grid: a grid
carries no direction, and direction is the whole content of a progression. The rail keeps every
level-select property the pattern earns its place on (discrete nodes, per-node state, one click to
enter) and adds the axis. On a phone the rail is also the layout a grid would have collapsed into
anyway.

## Left on the table, honestly

- **Pattern 4 (devlog feed) and pattern 5 (proof-of-play stills)** are not built. Both are content
  jobs rather than layout jobs: 4 needs DECISIONS.md entries curated by a human eye, and 5 needs
  harness stills copied into this repo and captioned with the fact each one demonstrates. They
  remain the two strongest unbuilt items and they are what "More About This Project" in
  `SITE-PLAN.md` should be made of.

## One thing this file could not have told the builder, learned the hard way

The flagship-visual gate greps **authored bytes for its own trigger words**, so a CSS comment reading
`/* ---- hero ---- */` on a block that is an `h1` and a paragraph blocks the write, and so does a
comment written to declare that the page contains no such visual. The fix is neither a workaround nor
a fake design-source citation: name the block for what it actually is (`/* ---- title block: an h1
and a paragraph, type only ---- */`) and say the rest in the packet. Worth knowing before writing a
page under these bounds.

## Sources

- [Game UI Database: Level Select / World Map](https://www.gameuidatabase.com/index.php?scrn=6)
- [Game UI Database: Stage/Level Select](https://www.gameuidatabase.com/index.php?scrn=42)
- [itch.io: Designing your page](https://itch.io/docs/creators/design)
- [itch.io: Introducing devlogs](https://itch.io/updates/introducing-devlogs)
- [Wikipedia: Start screen](https://en.wikipedia.org/wiki/Start_screen)
- [Medium: Loading Screens and Liminal Spaces (Franziska Fink)](https://medium.com/@franziska.fink.rpllc/loading-screens-and-liminal-spaces-why-we-miss-waiting-ab102aa328a1)
- [Adobe Express: Press Start microtrend](https://www.adobe.com/express/learn/blog/2025-microtrends-press-start)
- Repo-verified: `gamegote/docs/SITE-PLAN.md`, `gamegote/index.html`, `gamegote/desert/index.html`,
  `sand-bending/docs/DECISIONS.md` (W10-15, the generative OST entry)

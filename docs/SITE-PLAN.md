# gamegote.com: the progression site, gamified, with the deep-dive behind it

**Status: QUEUED, plan only, nothing built. Owner instruction 2026-08-01 ~9:50 PM PT (window 8).**
It queues behind the REPOSE to Gote Desert rename, which touches the same files and is already
decided. Doing this first would guarantee a conflict with it.

## One flag on the ask, resolved rather than left hanging

The message opens with "gotefigure.com" and then describes the milestone builds, older versions, the
project timeline and finishes on "when we're building gamegote.com". gotefigure.com is the apparel
store and has no versions, no milestones and no builds. **Read as gamegote.com.** One word from
Rotem overturns it.

## The ask, as given

> Another thing I want you to do is to improve the UX a bit on gotefigure.com. I think we have a lot
> of useful skills and agents, but I want you to give it a bit more of a gamified feel and then have
> a More Info About It tab where you can click. We dive more into the deep, deep details of how we
> actually get graphics, responsive lighting, and all of this in HTML. I think that'll be cool.
>
> For the actual site itself, we have the latest version at the top, and then you can click
> underneath and check out the older versions. The newest version should be at the top in one click.
> There's More About This Project, and then you can see a timeline progression. We have screenshots,
> maybe short screen recordings, clips, and info about the learnings of the project, important
> decisions, things that we learned, things like that, and decisions we made and code we made. Things
> that we're proud of that we want to show off, et cetera. It doesn't all have to be in a timeline.
> We just need to find a clean way to display it. We need to find appropriate skills and agents. I
> think we also have a skill for displaying work for clients. I think that could be useful for the
> way we display it in the More About It section. We have a lot of great UX skills which we should
> use when we're building gamegote.com.

## What it decomposes into

1. **Play the newest build in ONE CLICK, at the top.** Older milestones live underneath, reachable
   but never in the way. That is a hierarchy change, not a list.
2. **A gamified feel** on the hub and the project page.
3. **"More About This Project"**: the deep dive. How the graphics actually work, responsive lighting,
   all of it running in a single HTML file. Screenshots, short screen recordings and clips, the
   learnings, the decisions, the code worth showing off.
4. **A clean form for it, not necessarily a timeline.** His words: it does not all have to be in a
   timeline, it has to be clean.

## The material already exists, and that is the point

This project has been keeping the deep-dive content for eight windows without calling it that:

| what he is asking for | where it already lives |
|---|---|
| decisions, with the reasoning | `sand-bending/docs/DECISIONS.md` |
| what we learned, with the receipts | every `Reports/Sessions/2026-08-01 Window SAND Wrap N` |
| the graphics detail, measured | `sand-bending/PERF.md` (1921 lines), `docs/EARLYOUT-LADDER.md` |
| screenshots at delivered size | `sand-bending/harness/results/**/stills/` |
| clips, already named for what they show | `harness/results/m3-2026-08-01/clips/`, `CLIPS.md` |
| the milestone builds themselves | `repose/play/m0.html`, `m1`, `m2` (live) |

**Nothing here needs to be invented. It needs to be selected and staged.** The honest risk is the
opposite one: this repo generates far more evidence than a page can carry, so the work is curation,
and the failure mode is a wall of engineering nobody reads.

## Rules this build is bound by, before any of it starts

- **The gamified look is a FLAGSHIP VISUAL.** Vault HARD rule: it is designed in Claude Design plus
  the agreed tools, then landed in code through REPRODUCE. There is no from-scratch-in-code path for
  it. No approved design artifact means no build.
- **Load the skills before building**, which is his instruction and a standing rule after the
  window where four solved problems got rebuilt from scratch: `ux-visuals` (the flagship visual and
  scroll-craft arsenal), `client-presentation` (his "skill for displaying work for clients"),
  `impeccable` / `frontend-design` for the UI pass, `web-game-craft` for anything about the game
  itself.
- **The technicality default is LOW** (memory `keep-post-technicality-low-by-default`): a fact about
  the WORLD beats a fact about the CODE. The deep dive is the one place technicality is the point,
  which is exactly why it sits behind a click instead of on the hub.
- Deploy is Vercel, static, from `rotemgotlieb-dev/gamegote`. Any new route obeys the same
  folder-equals-URL convention the hub already uses.

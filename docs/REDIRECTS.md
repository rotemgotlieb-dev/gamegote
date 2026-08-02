# The /repose/ to /desert/ move, and why the redirects are shaped this way

Written 2026-08-01 with the rename. `vercel.json` is the whole mechanism; none existed before this
(no `vercel.json`, no `_redirects`, no route config anywhere in this repo), so these are new config
rather than an edit to something already load-bearing.

## The rule the owner set, in his words

> The old paths must 301, not 404. A rename that kills the old URLs breaks every link that already
> exists, including the milestone links on the page itself. A 404 is a deletion with extra steps.
>
> Redirect each play/mN.html to its own new target, not a catch-all to the index, or every deep link
> silently degrades to a landing page. Leave the redirects up permanently.

## Why every milestone gets its own line

The catch-all is the last entry, not the first, and the three milestone files are named explicitly
above it. That ordering is the whole point: **a deep link that silently lands on a landing page looks
like it worked.** Someone who bookmarked `/repose/play/m1.html` gets M1, not a hub where they have to
find M1 again and wonder whether it is the same build.

`/repose/:path*` sits underneath as a floor, so anything not enumerated (a path someone shared that
this repo has forgotten about) still lands somewhere sensible instead of 404ing.

## Why `permanent: true`

On Vercel, `permanent: true` emits **308** and `false` emits 307. The classic **301** is what a
permanent move is normally called and what the ask says, and 308 is its method-preserving equivalent:
both are permanent and both are cached and followed by browsers, search engines and link checkers
alike. The practical difference is that 308 forbids a client from rewriting a POST into a GET, which
for static pages never arises.

**Stated plainly rather than silently substituted:** these will answer 308, not literally 301. If a
literal 301 is required, Vercel's static config cannot emit one and it would take a middleware
function, which is a runtime where there is currently none. That trade is worth naming and is not
worth taking on its own.

## What was NOT renamed, on purpose

`desert/play/m0.html`, `m1.html` and `m2.html` still carry `<title>REPOSE</title>` inside them, and
that is deliberate. **They are the shipped milestone builds, byte-identical to the packs the
engineering record cites by cksum.** Rewriting a frozen artifact to fix its branding falsifies every
citation to it, and these three files are precisely the "older versions" the site is meant to
preserve. They are what those milestones were.

## Verifying, after a deploy

A deploy is a PUBLISH and therefore Rotem's click, never an agent's, so nothing here has been pushed.
Once it is live, this is the check, and it must be run without a pipe so the exit status is the
gate's own:

    for p in / /play/m0.html /play/m1.html /play/m2.html; do
      curl -sS -o /dev/null -w "%{http_code} %{redirect_url}\n" "https://gamegote.com/repose$p"
    done

Each line must show a 3xx and a `redirect_url` ending in the MATCHING `/desert$p`. A line that
redirects to `https://gamegote.com/desert/` for a `play/mN.html` source is the exact failure the
per-file entries exist to prevent.

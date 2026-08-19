# CLAUDE.md

Static tools comparing gravel/bikepacking framesets by **where they put the
rider**. Two pages, no build step, no dependencies:

- **`index.html`** — the shortlist. One shared cockpit fitted to every frame, so
  the numbers isolate the frame. Frame data is the `BIKES` array in the inline
  `<script>`. Everything below describes this page unless it says otherwise.
- **`terrel.html`** — the dial-in. Two frames only, the Tutto and a Terrel, each
  with **its own cockpit**, for speccing a build against the bike Joe rides. See
  *The dial-in page* below.

Read this before changing the model. Several things here were arrived at by
correcting mistakes, and they are easy to reintroduce.

---

## Why this page exists

Joe is **buying a frameset**. The page is a shortlist, not an analysis exercise.

The reference bike, the Cinelli Tutto Plus, is a **fixed-gear frame** — track
rear spacing, so wheels are a recurring problem and it was never built for a
derailleur. He built it up drop-bar on 40 mm tyres, likes how it rides and
handles, and is **keeping it as a fixie**. It is on the page as the shape he
already knows he gets on with, not as a bike to modify.

So: **never propose regearing, converting or otherwise fixing the Tutto.** It has
been ruled out repeatedly. The deliverable is always a *different frame*.

He has also ruled out going MTB — he watched the MTB-vs-gravel comparisons and
decided the cases where an MTB wins are too rare for his riding. Drop bars,
gravel tyres, gravel gearing. Weight that when the flat-bar-ish outliers here
(PBJ, Fenrir) come up.

---

## The framing, and the mistake to not repeat

The tool measures each frame against a **datum**, not a target.

The datum is a reference position. Frames are described by how far they land
from it — signed deltas in bar reach and saddle-to-bar drop. **Nothing is a pass
or a fail.**

The original version treated the datum as a hard constraint and reported
"impossible" or "front end too tall" when a frame's stack meant the bar sat
above it. That is a category error: a bikepacking frame is *designed* to put the
bars higher. A 50 mm drop is what a loaded touring bike is for; an 85 mm drop is
what a fast gravel bike is for. Both are descriptions of posture.

Concretely, do not:

- reintroduce a verdict/badge column
- describe a frame as unable to reach the datum
- phrase anything as the stem clamp needing to sit below the top of the head tube
- treat "closer to the datum" as "better"

If a future change needs a required-cockpit solve (spacers + stem length to hit
the datum exactly), that is fine as a *secondary* readout. It must not become
the primary verdict.

## Where the datum comes from

Source document: **Fairlight fit report, 9 Jan 2026** — `Bike geometries - Fit
Output - Joe Czar.pdf`, in the project root. Rider height 185 cm. Read it before
touching the datum. It carries Joe's name, email and body measurements, so it is
excluded from deploys via `.vercelignore` (`*.pdf`) — keep it that way.

The report has two halves, and the difference between them matters:

- **YOUR MEASUREMENTS** — saddle height 775, saddle setback 50, saddle centre to
  bar centre 545, drop from saddle to top of bars −80, bar width 400. **These are
  Joe's own measurements, taken off his Cinelli Tutto Plus.** They are a real
  ridden position, not a recommendation, and not a target. This is the datum.
- **OUR RECOMMENDATION** — Fairlight's proposed builds: 56R with a 110 mm stem,
  15 mm of spacers, −6°, achieving 544 / −79 (bar reach 488, bar stack 648); 58R
  with a 100 mm stem, 0 spacers, −6°, achieving 547 / −78 (491 / 649). These are
  the *check* the derivation reproduces, not the source of the datum.

Do not write, anywhere, that the datum is Fairlight's recommendation for a Faran
56R, or that it was never measured off Joe's own bike. Both were wrong and both
have already been corrected once.

Derivation, which reproduces Fairlight's achieved figures within 2 mm:

- Treat the 545 as a **straight-line** distance, not horizontal.
  `sqrt(545² − 80²) = 539.1` horizontal; saddle ref at x = −50, so bar centre
  x = 489.
- Bar clamp centre to bar top is ~15 mm (`BAR_TOP`). The report's 80 mm drop is
  measured saddle top to **bar top**, so saddle top = 648 + 15 + 80 = 743.
- Check: Fairlight's own output was 488/648 on the 56R and 491/649 on the 58R.

Two figures worth keeping from the report's notes: Fairlight build a Faran with
**40 mm of spacers by default**, and 40 mm is the **maximum** spacer height.

**The datum is the January 2026 position, and the bike has moved since.** Joe has
**flipped the stem on the Tutto** since the report was written, and runs **25 mm
of spacers**. Both are now on the Tutto's `cockpit` override (see below), so the
reference bike is drawn as it actually is. His current ridden position is
therefore higher and shorter than the datum. The datum stays as measured; it is
a dated reference, not a live reading. Say "as measured in January 2026", not
"what Joe rides".

**Open discrepancy — the frame data is settled, the January measurement is not.**
The datum came off the Tutto, so the Tutto row should reproduce it, and it does
not. Every frame-side candidate is now eliminated:

- The geometry is confirmed against Cinelli's own chart (see below).
- The stem is **80 mm** with drop bars — Joe confirmed it, Aug 2026. It is on
  the Tutto's cockpit override.
- The size is **L / 56**. Joe owns the bike and has confirmed this directly.
  **It is not the XL.** An XL/59 reproduces the report's numbers more closely on
  paper, and that fact is a coincidence, not evidence — do not raise it again,
  and do not "helpfully" re-derive it.

So the row lands at bar **466 / 636** against the datum's 489 / 663 — 23 mm
short, 27 mm low — and the remaining explanation is on the measurement side, not
the bike side. Joe's own account: the 545 / 80 in the report are **his hand
measurements and may simply be off**, and he has flipped the stem since. Two
ordinary ways a hand-measured drop goes wrong by this much: measuring the drop
to the hoods or the highest point of the tops rather than to the bar centre, and
the January spacer stack being different from today's 25 mm.

The datum still stays as written — it is a dated January 2026 reference, and it
is not moved to make a row agree. But its residual against the Tutto row is
**measurement uncertainty, not a modelling bug and not a data bug**. If a
current, carefully taken reading of Joe's position ever arrives, add it as a
second dated reference rather than editing the January one.

The saddle is modelled per-frame: fixed saddle-top height, so each frame's seat
angle decides the fore/aft position and the exposed post length. Earlier versions
plotted a single floating point 50 mm behind the BB, which was wrong — that 50 is
measured to a point near the saddle nose, not the sit bones.

## The 30 mm overhead is calibrated, not guessed

The cockpit input is **spacers**, plus a separate `overhead` for headset top cup
and half the stem clamp. Zero spacers still leaves the clamp well above the head
tube.

**The shared cockpit defaults to Joe's own**: 25 mm of spacers, 90 mm stem,
flipped to **+6°**. Every frame is tried with the setup he actually rides, not
with an arbitrary slammed one. These live in two places that must stay in step —
the `value` attributes on the inputs and the `cock` initialiser. They were out of
sync once and the page silently rendered numbers that didn't match its own
inputs.

Calibration: hold Fairlight's builds exactly as specced (56R, 110 mm stem, 15 mm
spacers; 58R, 100 mm, 0 spacers) and solve for the overhead that reproduces the
bar coordinates their report says those builds achieve — 488/648 and 491/649.
That gives **36.66 mm and 36.76 mm**, the same constant twice to a tenth of a
millimetre, matching a Faran's 17 mm Hope Cup 1 plus a stem clamp. Frames with a
low-stack cup (8 mm) sit nearer 27 mm, so 30 is the default.

Do not re-derive this by solving stem *and* rise against the datum and then
subtracting Fairlight's spacers — that mixes a solved stem with their spacer
stack and lands at 36.2–36.4, which is what the page used to say.

Independent check: the solver asks for 26 mm rise and a 90 mm stem on a Grizl CF
L. Canyon specs that bike with 22.5 mm of spacers and a 90 mm stem.

## Aero bars are a secondary readout, and must stay one

Behind the `Aero bars` toggle: the same clip-ons fitted to every frame, exactly
like the shared stem. `aero = { rise, ext, tilt }` — pads `rise` above the bar
top, hands `ext` out along extensions tilted `tilt` degrees up. Defaults 45 / 200
/ 10. `fit()` returns `pad`, `ext`, `padDrop`, `extDrop`; the table and the
drawing are both hidden until the toggle is on.

Pads are modelled straight above the bar clamp centre. Real clip-ons put them
±20 mm of that, which moves the hands more than the frame differences do — so
this is a starting position, not a fitted one, and the note says so. Keep it
saying so.

Same rule as everything else here: no verdict. A frame whose extensions land
above the saddle is describing a bikepacking posture, not failing anything. The
reason the readout exists is the PBJ — a frame whose grips sit high and ~100 mm
back once you account for the bar's 28° sweep and −112 mm reach, where clip-ons
are what restore a usable long position rather than an add-on.

Do not change the default without redoing this check.

## Fork offset

If a `BIKES` entry omits `rake`, offset is **derived** by closing the frame's own
published wheelbase, chainstay, BB drop, head angle, stack, reach and head tube
length. This reproduces the published offset within 1 mm on every frame that
prints one (8bar 49.3 vs 50, Salsa 51.5 vs 51, Santa Cruz 51.5 vs 51.5).

It also supplies the Canyon Grizl figure (~43 mm), which Canyon does not publish
— hence its comparatively low trail. Flagged in the page footer.

Prefer published `rake` when available; the Faran (60) and Sklar (55) both have
it set explicitly because their large offsets are the whole point.

**`fc` beats `wb`.** The front axle normally comes from the published wheelbase.
Where a maker prints **front centre** — BB to front axle, straight line — give
the entry `fc` instead and the axle is placed from that, with the wheelbase
recomputed and displayed from it. Front centre is the more reliable of the two:
on the PBJ it closes against Sklar's own published offset where their wheelbase
misses it by up to 11 mm. Check both before choosing; if they agree, either is
fine.

## Data provenance and caveats — keep these visible

- **ROSE Backroad CF L** — **fixed, Aug 2026, and no longer estimated.** The old
  row had current-gen stack/reach with the previous generation's angles, seat
  tube, head tube and chainstay, because Rose serve their own table via
  JavaScript. The current-generation chart is mirrored at
  `geometrics.mtb-news.de/bike/rose-backroad-cf-2026.2247` — curl it, Rose's site
  is not needed. Corrections were substantial: head angle 72 → **71.5**, seat
  angle 73.5 → **73.75**, head tube 175 → **167**, seat tube 550 → **531**,
  chainstay 434 → **430**, BB drop 76 → **78**, wheelbase 1055 → **1064**, plus a
  published **48 mm offset** where the row had been deriving 40. Closing the new
  wheelbase gives 48.3 against that 48, and the ETT falls out at 590.5 against a
  printed 590, so the row is now fully self-consistent. `est` and the `*`
  footnote are gone.
  **This mattered:** the bad data put its trail at 74, close to the Terrel's 78.
  Corrected it is **68.5** — ten millimetres lower, near the Tutto. Joe caught it
  by noticing the row claimed *less* rake than the Terrel while showing similar
  trail, which is the signature of a mismatched head angle. Treat that pattern as
  a data smell, not a curiosity.
- **Bombtrack Hook EXT L** — these are the **titanium 27.5"** figures. The steel
  700C Hook's chart was unobtainable (bombtrack.com blocks automated access).
  Footnoted `**`. Shared platform, but BB drop and stack will differ.
- **Cinelli Tutto Plus L (56)** — **confirmed against Cinelli's own numbers**,
  Aug 2026. Their site renders the geometry table from a `GEOMETRY_DATA` JSON
  blob inline in the product page HTML (`/products/tutto-plus-*`), not from an
  image — curl the page and pull it out; the rendered page and every database
  mirror is downstream of it. Every figure the row already had matches exactly:
  stack 536, reach 410, ETT 560, seat tube 560, head tube 110, 72° / 74.5°,
  BB drop 60, chainstay 425. The odd per-size head angles (71.5 / 72.5 / 72 /
  73.5 for S/M/L/XL) are **Cinelli's own published progression**, not a database
  artifact — strange, but no longer a reason to distrust the row.
  Cinelli also publish **rake 45** and **front centre 616**, and 616 closes
  against 45 to within 1 mm where the printed **wheelbase 1024 wants 37** — the
  same conflict as the Sklar PBJ, resolved the same way. The entry therefore
  carries `fc:616` and `rake:45`, and the wheelbase shown is the derived 1034.
  Trail is 65, not the 74 the old derived offset gave. Do not revert to `wb`.
  Frameset facts that matter: Columbus Cromor, **rear QR 130 mm** with track
  ends plus a derailleur braze-on, **V-brake pivots** (rim brake, no disc
  mounts), 45 mm tyre clearance, fork rake 45. Stock build is flat-bar with a
  **100 mm stem** on the L; Joe's is his own drop-bar build, so the stem on the
  page is a guess either way — see the open discrepancy above.
  **Stock spacers, Aug 2026 research: Cinelli publish none.** Their spec table
  gives stem (6061, 31.8, 6°, H 40 mm, 90 S/M, 100 L/XL), bar, headset
  (EC34/28.6 | EC34/30, external cup) and seatpost, and stops there — no spacer
  count, no steerer length; the fork ships uncut (1150 g). Scaled off Cinelli's
  own side photo of the current flat-bar build and Sigma Sports' 2020 photo,
  using spacer OD ≈ 30 mm and a 72° axis for the tilt, the stack above the top
  cup measures **26–33 mm** on both — call it **~30 mm, ±5**. For 2020 Sigma also
  sold a factory **drop-bar** Tutto Plus ("Pista Drop Bar", Tifosi Gravel bar,
  Cinelli Alu stem, Cinelli SSM Monza saddle) — they never printed its stem
  length.
  **Joe's own stack is 2×10 + 3×5 + 1×2.5 = 37.5 mm of spacers**, and where they
  sit has changed (Aug 2026, from him): in the photo **two 5s sat above the stem**
  (27.5 below), and **now only the 2.5 is above** — so the figure that belongs in
  `cockpit.spacers` is **35**. Anything above the stem clamp raises nothing and
  must not be counted.
  Photo measurement, as a method worth reusing: de-rotate the head-tube crop
  about the steering axis (17° from vertical in that frame), scale by spacer
  OD = 30 mm, read boundaries off the axial profile. That gave **31 ± 3 mm**
  against the true 27.5 — usable to about ±4 mm, not better. It resolves the top
  of the headset's shiny ring and the head tube top 10.5 px below it (cup stack
  ≈ 12 mm, a normal EC34 figure — that is the scale check), then the step out to
  the stem clamp; the seams inside the stack read as 10 + 10 + 5 + ~5, so
  individual spacers under 5 mm are not separable.
- **Sklar PBJ M / L** — Sklar's own chart, footnoted `†`. Offset 52 is published.
  Two of Sklar's own numbers disagree, and it is settled: closing the published
  **wheelbase** wants 63 mm of offset on the M and 56 on the L, but closing the
  published **front centre** (730 M, 755 L) gives 52.1 and 53.2 against their
  stated 52. The front centre wins, so both entries carry `fc` instead of `wb`
  and the wheelbase shown is the derived 1153 / 1179, not Sklar's printed
  1165 / 1182. Do not revert this to `wb` — the earlier chainstay-range theory
  (432 is a minimum, so the wheelbase must be quoted elsewhere in the range) was
  a guess, and the front-centre figure disproves it.
  No standover in the source; left blank. It is a rigid 29er on a 29 × 2.6
  reference tyre (754). Sklar design it around a **30–50 mm stem** (35 on stock
  builds) plus a swept-back riser bar, so under this page's shared 90 mm stem its
  bar reads roughly 55 mm longer than it would ride. Sklar themselves say a drop
  bar on a PBJ is hard to fit well and point drop-bar riders at the
  SuperSomething.
- **Sklar's fit windows** — Sklar size by saddle height *and* saddle-to-bar, the
  same two numbers as the datum, which makes them directly checkable. PBJ L is
  saddle height 730–790 and saddle to stem 530–570; the datum's 775 / 545 sits
  mid-window, and the M is outside both. SuperSomething L is 730–795, its M-L
  tops out at 750 — so `sklarML` is below the datum's range and is on the page
  for shape comparison, not as a candidate. Sklar measure saddle *tip* to stem
  centre; the fit report measures saddle to bar. Close, not identical.
- **Standert Erdgeschoss 58 / 60 ‡** — Standert's own chart (stainless frameset
  page), which prints no BB drop. The fork rake is selectable 47 or 52 mm;
  closing the published wheelbase against 52 wants **75 mm drop**, and 75 then
  reproduces 52.0 on the 58 and 51.5 on the 60. Keep 75. The 425 chainstay is
  the short end of a sliding dropout. Steep head angle plus long rake is a
  deliberate low-trail choice — it lands near the Faran, which is the point of
  having it here.
- **Standert Kreissäge RS 56 / 58 ‡** — same chart source, but neither drop nor
  offset is published. Drop is **assumed 70** (road standard), which derives
  43.5–43.9 offset. Flagged as an assumption in the footnote. 35 mm max tyre, so
  it is drawn on 686 (700×32), not the 712 everything else uses.
- **Otso Fenrir Stainless L §** — Otso's chart via their geometry sheet. Every
  wheel-related number is a range because of the Tuning Chip dropout (chainstay
  430–450, drop 68–72, wheelbase 1150–1170, ±0.25° on both angles). The row is
  the **middle setting**, 440/70, which closes wheelbase 1150 against the
  published 51 mm offset. Stack assumes the 495 mm suspension-corrected rigid
  fork — not like-for-like at the head tube. Same short-stem caveat as the PBJ.
- **CUBE Nuroad C:62 L / XL** — CUBE's own chart (via mtb-news geometrics; Geometry
  Geeks' entry for this bike is scrambled, don't use it). No footnote: the chart
  cross-checks internally — the published effective top tubes (575 / 592) fall out
  of stack, reach and the 73° seat angle exactly, and closing the wheelbase derives
  47.3 / 47.0 mm of offset, consistent across sizes. CUBE publish no offset, so it
  is derived like the Grizl's. L is the size for 185 cm (CUBE list 183–189); XL is
  188–196 and is on the page for comparison.
- **BMC URS 01 L / M** — BMC's own chart (retailer PDF of the URS 01 THREE
  sheet), and it closes better than anything else here: the published 665 mm
  front centre derives **45.1 mm** of offset against their stated 45, and their
  1081 mm wheelbase falls out of it exactly. Head angle is 70° and rear centre
  425 on every size. No standover in the sheet, so it is left blank.
  **BMC print trail 77, the page says 82** — not a conflict: theirs is on the
  narrower tyre they spec, the page's is on the shared 45 mm reference. Trail
  moves with tyre diameter, so it is only comparable when the tyre is held
  constant, which is the whole reason the page normalises it. L is 178–188 cm,
  M is 172–180; the M's short 459 seat tube leaves over 300 mm of post out at
  Joe's saddle height. Clearance is 45 mm on this generation, 47 on the newest,
  against the Terrel's 50. It carries BMC's MTT — 10 mm of rear micro-travel
  through the seatstay junction — which the model does not represent.
- **Propain Terrel CF** — **confirmed against Propain's own chart**, Aug 2026:
  every figure in both rows matches, including the published 45 mm offset and
  72 mm BB drop, so nothing here is derived. The fork is **425 mm long and the
  geometry is suspension-corrected** for a 40 mm fork — but unlike the Fargo,
  Propain ship a rigid fork of that same length, so the published stack is what
  you actually get and the row *is* like-for-like. Propain's sizing is
  M 175–185 cm, L 183–193, and their own advice for someone between sizes is
  smaller for playful, larger for smooth. Note the head angle **slackens as the
  frames get smaller** (M 70.5° against L 71°), so sizing down here adds trail
  rather than removing it. 50 mm tyre clearance; they spec short stems across
  the range, so under the shared 90 mm the bar reads longer than it rides.
- **Salsa Fargo** — published stack assumes a 483 mm suspension-corrected fork.
  Not like-for-like at the head tube against rigid frames.
- **Santa Cruz Stigmata** — rider-height ranges are not in the source data.
  Omitted rather than guessed. Do not fill them in from memory.

## The dial-in page

`terrel.html`, added Aug 2026, once Joe had settled on the Terrel. Different job
from the shortlist: not "which frame", but "what stem and spacers on *this*
frame". So it holds **two cockpits** — `cockA` for the Tutto, `cockB` for the
Terrel — where `index.html` holds one shared one plus per-frame overrides.

- The geometry rows and the whole `fit()` model are **copied, not imported**.
  That is deliberate for a static page with no build step, and it is the one real
  hazard here: **a geometry correction has to be made in both files.** The
  Terrel and Tutto rows must stay identical between them.
- Saddle height is shared, because it is the rider, not the frame.
- Panel B opens as a copy of panel A — Joe's own parts moved across — so the
  first thing the page says is what bolting his cockpit onto a Terrel would do.
- Three solves, all closed-form. `spacersFor()` gives the spacer height that
  lands the bar at a target height for a given stem and angle. `barXat()` gives
  the reach that results. `solveBoth()` solves stem *and* spacers together for an
  exact bar position; the determinant works out to cos(angle), so it only
  degenerates at a meaningless ±90° stem.
- **Negative solve results are kept and shown**, never clamped away in the
  readout, because a negative figure is the useful description of how much higher
  the frame's own front end already sits. The *buttons* clamp at zero spacers and
  say what that leaves. Do not phrase any of this as the frame failing to reach
  something, and do not talk about the clamp sitting below the head tube — the
  no-verdict rule from the shortlist page applies here in full.
- The dial-in table's insight, worth not losing: once bar **height** is matched,
  reach moves with stem length and barely at all with stem angle (under a
  millimetre across the whole range). So reach is a stem-length decision and
  height is a spacers-and-angle one, and the table is laid out to show that.
- The Tutto's bar height gets its own rule on the drawing, parked at a different
  x from the datum label because the two land within 20 mm of each other.
- **The Terrel's overhead is 40, not 30.** Its cables pass under the stem into the
  headset — Propain call it a semi-integrated cockpit — and Joe puts that routing
  cover at **10 mm** below the clamp (his figure, Aug 2026). So panel B defaults to
  `overhead:40` where the Tutto keeps 30, and *Your parts as they are* deliberately
  carries spacers, stem and angle across but **not** the overhead: that belongs to
  the frame's headset, not to the parts. Panel B also opens on **10 mm of spacers**
  rather than none — a build choice, not a frame fact, because spacers you can
  remove are the only way to lower this bar without shortening hoses.
  **`index.html` still fits the Terrel with the shared 30**, so its Terrel bar
  figures read about 10 mm low. Fixing that properly means letting a frame carry
  an `overhead` in its `cockpit` override, which cuts against the rule that only
  the reference bike overrides anything — Joe's call, not settled.
- **State persists in `localStorage`** under `bike-geo.terrel.v2` (v1 was the
  pre-routing defaults; the key was bumped so the new ones actually appear) — every control,
  including which size and the two toggles. `sessionStorage` was considered and
  rejected: the point is to come back to a half-dialled build another day. Rules
  that matter if you touch it: everything loaded is validated through
  `cleanCock()`/`num()` and falls back to the default rather than throwing, since
  a bad stored value would otherwise be invisible and sticky; `save()` and
  `load()` are both wrapped in try/catch so private mode degrades to forgetting
  rather than a blank page; `sync()` drives *every* control from state, so the
  `value` attributes in the HTML are only the pre-JS fallback here — unlike
  `index.html`, they are not the source of truth. The **Reset** button clears the
  key and re-saves the defaults. Storage is per-origin, so a `file://` copy and a
  deployed copy keep separate state.

## Adding a frame

Append to `BIKES`:

```js
{ id:'shortid', brand:'Maker', name:'Display Name', color:'#hex', on:false,
  stack, reach, ett, st, ht, hta, sta, drop, cs, wb, standover, tire, rake }
```

- `brand` is **required**. Chips are grouped by it — one row per maker, the
  reference bike first and the rest alphabetical — with a filter box above.
  Thirty-plus frames in one flat row was unusable.
- Start `name` with the brand exactly as `brand` spells it: the chip strips that
  prefix so it reads as the size within its group, and falls back to the full
  name if the two don't match. The tables and the drawing still use the full
  name.

- `ett` may be `null` — it is derived as `reach + stack/tan(sta)`.
- `tire` is the reference tyre **diameter** in mm (700x45 ≈ 712, 29x2.25 ≈ 736).
  Ground lines differ between frames because reference tyres differ; that is
  intentional, everything else is measured from the shared BB.
- `rake` optional — see above.
- `fc` optional, and replaces `wb` when the maker publishes front centre — see
  above. Give one or the other, not both; `fc` overwrites `wb`.
- `ref: true` draws the frame dashed and thinner.
- `cockpit: { spacers, angle, stem }` optional, and overrides the shared cockpit
  for that frame only — `cockpitFor()` merges it over `cock`. **Only the
  reference bike should have one.** The Tutto carries
  `{ spacers:25, angle:6, stem:80 }` because it is Joe's actual bike and belongs
  on the page as it really is — he confirmed the 80 mm stem and drop bars in Aug
  2026; every candidate frame must keep the shared cockpit, or the comparison
  stops isolating the frame. Spacers and angle match the shared defaults, so
  those do nothing until the sliders move, but the **stem does not** — the
  shared default is 90, so the Tutto is drawn 10 mm shorter than the candidates
  from the start, which is correct and is why the caption calls it out. Move the
  sliders and the Tutto stays pinned to reality while the candidates follow,
  which is the point. The caption names an overriding frame
  only when its cockpit actually differs from the current shared one.

Presets are hardcoded id lists in the button handlers near the bottom.

## Drawing conventions

- Aligned at the bottom bracket, y up in bike coordinates, flipped in `Y()`.
- Solid lines are tubes **as built** — the top tube runs from the seat tube
  collar to the top of the head tube at its real slope. An early version drew
  the horizontal *effective* top tube as if it were the tube itself, giving every
  frame a fake level top tube. Don't.
- Dashed lines are chosen parts and measuring conventions: seatpost to the
  required saddle height, steerer rise plus stem out to the resulting bar
  position, and the horizontal effective top tube as reference.
- The **steering axis is carried down to the ground** as a faint dotted line, and
  the **trail** it leaves is the thick tick along the ground line from the tyre
  contact point forward to where the axis lands, labelled with the figure. This
  exists because two frames can be the same size in every dimension a tape
  measures — the Faran 58R and Terrel CF M are within 2 mm on reach, front
  centre, chainstay and standover — and still be 29 mm apart on trail, one
  reaching it with a steep head angle and a 60 mm rake, the other with a slack
  head angle and 45. Without this, that difference is invisible on the drawing.
- Number labels **nudge each other down** (`labelY`) when frames land on top of
  one another; `PAD.b` is sized to hold a stack of trail figures. Frames that
  land in the same place are the normal case here, not the exception.
- Colours must stay **distinguishable between frames that get compared**. The
  Terrel M was cream and sat invisibly on top of the Faran's cream; it is now
  pink like the rest of its family. Check a new colour against the frames it
  will be drawn with, not just against the list.
- `Axle ahead of hands` = front axle x minus bar x. This is the metric that
  answers "how does the slackness play at this size" — sizing down shortens front
  centre but does not change head angle or trail.

## Design

Dark technical-drawing palette on `--ink: #12161a`. Archivo / Archivo Narrow for
labels, JetBrains Mono for all numbers, tabular-nums throughout. Hairline rules,
2px radius, no shadows. Keep numbers monospaced and right-aligned; the tables are
spec sheets, not marketing.

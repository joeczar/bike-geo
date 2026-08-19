# Gravel frame posture comparison

Static tools comparing gravel/bikepacking framesets by **where they put the
rider**. No build step, no dependencies. Fonts load from Google Fonts at runtime.

Two pages:

- **`index.html`** — the shortlist. One shared cockpit (spacers, stem length,
  stem angle) fitted to every frame, so the numbers isolate the frame rather
  than the parts. Frame data is the `BIKES` array in the inline `<script>`.
- **`terrel.html`** — the dial-in. Two frames only, each with its own cockpit,
  for solving stem and spacers against a build. State persists in
  `localStorage`.

Frames are described by signed deltas from a reference position — bar reach and
saddle-to-bar drop. Nothing is a pass or a fail: a 50 mm drop is what a loaded
touring bike is for, an 85 mm drop is what a fast gravel bike is for. Both are
descriptions of posture.

## Deploy

```sh
npx vercel        # preview
npx vercel --prod # production
```

Or drag this folder onto the Vercel dashboard. `vercel.json` sets cache headers
only.

## Adding a frame

Append to `BIKES`:

```js
{ id:'shortid', brand:'Maker', name:'Display Name', color:'#hex', on:false,
  stack, reach, ett, st, ht, hta, sta, drop, cs, wb, standover, tire, rake }
```

- `ett` may be `null` — derived as `reach + stack/tan(sta)`.
- `tire` is reference tyre **diameter** in mm (700x45 ≈ 712, 29x2.25 ≈ 736).
- `rake` optional. Omit it and fork offset is derived by closing the frame's own
  published wheelbase — within 1 mm of the published figure on every frame that
  prints one. Prefer a published `rake` when there is one.
- `fc` optional, and replaces `wb` where the maker publishes front centre. Front
  centre is the more reliable of the two; the wheelbase is then recomputed from
  it.

`CLAUDE.md` carries the full model notes — where the reference position comes
from, how the cockpit overhead was calibrated, and the per-frame data
provenance. Read it before changing the model.

## Data caveats

- **ROSE Backroad CF L** — current-generation chart throughout, cross-checked
  against the published offset and effective top tube.
- **Bombtrack Hook EXT L** — titanium 27.5" figures standing in for the steel
  700C, whose chart is unobtainable. Shared platform, but BB drop and stack will
  differ.
- **Sklar PBJ M / L** — Sklar's own chart. Their wheelbase and front centre
  disagree; the entries use front centre, which closes against their published
  52 mm offset. No standover or rider heights published. Rigid 29er designed
  around a 30–50 mm stem, so under the shared 90 mm its bar reads much longer
  than it rides.
- **Standert Kreissäge RS** — BB drop assumed 70 mm (road standard); no drop or
  offset published.
- **Otso Fenrir Stainless L** — Tuning Chip dropout makes every wheel-related
  figure a range; the row is the middle setting. Stack assumes the 495 mm
  suspension-corrected rigid fork.
- **Salsa Fargo** — published stack assumes a 483 mm suspension-corrected fork,
  so it is not like-for-like at the head tube against rigid frames.
- **Canyon Grizl, CUBE Nuroad** — makers publish no fork offset; it is derived.
- **Santa Cruz Stigmata** — rider-height ranges not in the source data, left
  blank rather than guessed.

Trail is computed on each frame's own reference tyre, so it will differ from a
maker's printed figure where they spec a narrower tyre.

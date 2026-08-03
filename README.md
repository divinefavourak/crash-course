# INS 202 — HCI Crash Course

A single-file study app for INS 202 (Human–Computer Interaction, University of Lagos).
217 practice questions and revision notes across lectures L1–L8.

Everything lives in `index.html`. No build step, no dependencies, no server-side code —
open it directly or drop it on any static host.

## Run it locally

Double-click `index.html`, or serve the folder:

```
python -m http.server 8000
```

then open <http://localhost:8000>.

## Deploy it

Any static host works. The whole site is one file.

**GitHub Pages**

```
git init
git add .
git commit -m "INS 202 crash course"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Source: `main`, folder `/ (root)`**.
It publishes at `https://<you>.github.io/<repo>/`.

**Netlify** — drag this folder onto <https://app.netlify.com/drop>.

**Vercel** — `npx vercel` from this folder, or import the repo.

**Cloudflare Pages** — connect the repo, leave the build command blank,
set the output directory to `/`.

## Notes on the build

- **Fonts.** The design calls for [Quicksand](https://fonts.google.com/specimen/Quicksand).
  The CSS tries it first and falls back to the nearest geometric-rounded system faces.
  To make it exact, either add the Google Fonts `<link>` in `<head>` (fine on your own
  host — there is no CSP restriction here), or download `quicksand.woff2` and inline it
  as a `@font-face` `data:` URI to keep the file self-contained.
- **Background.** The organic blobs are generated on a `<canvas>` from a seeded PRNG,
  so each screen composes its own layout and redraws identically on resize.
- **Progress.** Best score per module is kept in `localStorage`, so it is per-browser
  and never leaves the device.

## Editing the questions

The bank sits in the `<script>` block near the bottom, one call per question:

```js
q(module, "Question text", ["A", "B", "C", "D"], correctIndex, "Why this is the answer");
```

`module` is 1–8 (matching L1–L8) and `correctIndex` is 0-based. Option order is
shuffled at runtime, so answer position is never a tell. Counts in the menu are
derived automatically — add a question and the module card updates itself.

Notes for each module live in the `NOTES` object just below the bank, keyed 1–8.

Deployment Status: [![Netlify Status](https://api.netlify.com/api/v1/badges/fe9b198f-77af-4fa9-9967-c34e8b51b6ee/deploy-status)](https://app.netlify.com/projects/ins202/deploys)
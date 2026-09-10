# LoFi Layers AR Manager

A multi-target image AR site built with MindAR + Three.js, plus a
no-code manager page for adding AR prints yourself — no JavaScript
editing, no Node.js, no build tools, no server backend.

## How it works

- **`manager.html`** — the admin page. Add one entry per AR print (name +
  target image + video). When you click **Build AR Site**, it compiles all
  your target images into a single `targets.mind` file, matches each one to
  its video in `assets/config.json`, and downloads a complete, ready-to-host
  zip.
- **`index.html`** — the AR viewer. It reads `assets/config.json` at load
  time and builds one MindAR anchor + video plane per target, so it works
  for 1 print or 50 without any code changes. This is the same MindAR/Three.js
  approach as the original single-target build (anchor per target, video
  texture on a plane sized to the video's aspect ratio, play on found /
  pause on lost) — it's just looped over however many targets are in the
  config instead of hardcoded to one.

Everything compiles and zips **in your browser**. Nothing is uploaded
anywhere.

## Step 1 — Run the manager

Open this folder in VS Code and start **Live Server** on `manager.html`
(right-click → "Open with Live Server"). It needs to run through a local
web server, not be double-clicked — the page will warn you if you open it
the wrong way.

Alternatively, from this folder:

```bash
python3 -m http.server 8080
```

then visit `http://localhost:8080/manager.html`.

## Step 2 — Add your AR prints

For each print, click **+ ADD AR PRINT** and fill in:

- **Name** — shown on-screen when the print is recognized (e.g. "FOUND:
  Super Shredder").
- **Target Artwork** — the PNG/JPG of the artwork to recognize. High
  contrast, detail-rich images track best; flat/blank artwork tracks poorly.
- **Animation** — the MP4/WebM that plays over that artwork once recognized.

The order of the entries becomes the target index (Target 0, Target 1, …) —
this is also the compiled order inside `targets.mind`.

## Step 3 — Build

Click **BUILD AR SITE**. This:

1. Compiles every target image into one `assets/targets.mind` file, in
   entry order.
2. Writes `assets/config.json`, mapping each target index to its name and
   video path.
3. Copies each video into `assets/videos/<index>.<ext>`.
4. Packages `index.html` + `assets/` into `lofi-layers-ar-site.zip` and
   downloads it.

Compiling can take anywhere from a few seconds to a minute or so per image
depending on size and how many targets you have — the progress line will
update as it works.

## Step 4 — Deploy

Unzip `lofi-layers-ar-site.zip` and push its contents to GitHub Pages (or
any HTTPS host — Netlify, Vercel, Cloudflare Pages all work). Camera access
requires HTTPS (or `localhost` for local testing), same as before.

On the deployed site, point your phone's camera at any of the printed
target images. The banner reads "SEARCHING FOR AR PRINT…" until it
recognizes one, then "FOUND: `<name>`" while that video plays over the
artwork, pausing when it's lost from view again.

## Notes

- `manager.html` reads its own `index.html` as a template when building, so
  don't rename or remove `index.html` from this project folder.
- This project's own `assets/` folder starts empty — it's only ever
  populated inside the zip that `manager.html` generates. Don't expect this
  source folder to work as an AR site on its own; always deploy the built
  zip.
- To add, remove, or swap AR prints later, just re-run the manager and
  build a new zip — there's no in-place editing of a live site.

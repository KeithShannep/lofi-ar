# LoFi Layers AR Starter

A minimal image-target WebAR page built with MindAR + Three.js.

## What it does

1. Opens the phone camera.
2. Recognizes one compiled target image.
3. Plays `assets/animation.mp4` aligned over the recognized print.
4. Pauses the animation when the target is lost.

## Files you need to add

Place these two files in the `assets` folder:

- `targets.mind` — compiled MindAR image target file.
- `animation.mp4` — the animation that should play over the target.

## Compile your target image

Use the official MindAR image target compiler in the MindAR docs/studio. Compile one artwork image and export it as `targets.mind`.

The first image you compile is target index `0`, which is what this starter page uses.

## Local testing

Camera access normally requires HTTPS, except on localhost.

From this project folder you can run:

```bash
python3 -m http.server 8080
```

Then test on the same computer at:

```text
http://localhost:8080
```

For testing on a phone, deploy the folder to an HTTPS host such as Cloudflare Pages, Netlify, Vercel, or GitHub Pages.

## Next step

Replace the placeholder files with one real LoFi Layers target image and its matching animation. Once that works, expand this into a multi-target library and admin uploader.

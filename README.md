# $WOOPY OS

Retro desktop-style site for **$WOOPY — Dog wif Propeller** (Solana).

Live: https://tayanna.github.io/whoopy-os/

Plain static site: one `index.html` plus an `assets/` folder. No build step, no frameworks, no backend.

## Run locally

Open the folder in a terminal and start any static server:

```bash
python3 -m http.server 8000
# or
npx serve .
```

Then open http://localhost:8000

Opening `index.html` by double-click (file://) mostly works too, but use a server so video, fonts and the Jupiter widget behave like in production.

## Deploy

Any static hosting works: GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3. Upload the folder as is. The site must be served over **https** for the Jupiter wallet connection to work.

When moving to a new domain, update the absolute URLs in `<head>` (`og:image`, `og:url`, `twitter:image`) and the `logoUri` in `initJup()`.

## Structure

```
index.html            markup, styles and script (all in one file)
assets/
  bg.mp4, bg-poster.jpg     background video + its first frame
  avatar*.png/jpg           avatar (about window, window icons)
  favicon.ico, favicon-32.png, apple-touch-icon.png
  og.jpg                    link preview image (1200×630)
  dog-150x150.png           cutout dog (desktop icon, marquee, game sprite)
  dog.png, dog-lollipop.png, propeller-dog.jpg, giphy.gif   gallery / boot screen / grain overlay
  theme.mp3                 radio track
  fonts/                    Pixelify Sans + Roboto (woff2, self-hosted)
```

## Where to change things

| What | Where in `index.html` |
|---|---|
| Contract address | `const CA = …` in the script, plus the `<code id="ca">` text in the Buy window, the short text in the Contract widget and the pump.fun link |
| Social / platform links | the `<a class="ico" href="…">` desktop icons (X, pump.fun, FOMO) and the buttons in the Woop woop window |
| Radio tracks | `const tracks = [...]` (title, subtitle, file, start second) |
| Gallery images | `const art = [...]` (image path, caption) |
| Jupiter swap widget | `initJup()`: input/output mint, branding. Colours via `--jupiter-plugin-*` CSS variables |
| Texts | the `<section class="win">` blocks (Woop woop, Buy, Creatives, Game) |
| Colours / fonts | CSS variables in `:root` at the top of `<style>` |

## How it works

- **Windows** (`.win`): draggable by the title bar, minimise / maximise / close, double-click the title to maximise. Clicking a desktop icon opens a window (`data-open="…"` → `#w-…`).
- **Scaling**: all UI lives in `#ui`. `fit()` scales it up on large monitors and portrait tablets (max ×1.8). Below 860 px the mobile layout is used: icon grid on top, windows centred over a dimmed backdrop (tap outside to close), compact radio + contract at the bottom.
- **Boot screen**: the first click starts the music (browsers block autoplay with sound).
- **Buy**: the Jupiter Plugin (`plugin.jup.ag/plugin-v1.js`) is loaded on first open of the Buy window. jup.ag itself cannot be embedded in an iframe, the plugin is the official embed.
- **Game** (Propeller Run): small canvas game, best score kept in `localStorage`.
- `prefers-reduced-motion` turns off all animations.

## Notes

- The dog's T-shirt in several images (avatar, og.jpg, gallery) shows a Minion print, a copyrighted character. Replace before a wide launch.
- GitHub Pages caches pages for ~10 minutes; hard-refresh (Cmd+Shift+R) to see updates.

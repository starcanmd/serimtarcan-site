# A single-file personal site

A personal homepage that changes with the time of day and leaves soft brush strokes under the cursor. It is one HTML file. There is no framework, no build step, and nothing to install. Plain HTML, CSS, and JavaScript.

You are looking at this because you want to make it your own. This README walks through what the pieces are and exactly where to change them. The live original is at serimtarcan.com, but everything below is written for you adapting it, not for that specific deployment.

---

## What it does

- Three time-of-day phases: **dawn**, **midday**, **night**. The visitor lands on the one matching their local clock, and can switch by hand with the toggle in the corner.
- Each phase is a single background color that cross-fades smoothly when the phase changes.
- Moving the cursor lays down soft brush strokes that blend into the current phase color and fade out.
- Optional ambient music, one piece per phase, off until the visitor turns it on.
- A social-share card and favicon are included.
- It respects `prefers-reduced-motion` (skips the brush strokes for people who ask their system to reduce motion).

---

## Files

```
index.html      the entire site: structure, styling, and behavior
favicon.svg
og-image.png    social-share card, 1200x630
audio/          three audio files, one per phase
.gitignore
```

Everything lives in `index.html`. The sections below tell you which part to search for.

---

## Make it yours

Open `index.html` and edit in place. Use your editor's find (Ctrl/Cmd-F) to jump to each landmark.

### Your name and the accent letter
Search for `class="wordmark"`. The big name is there, written as `Ser<em>i</em>m`. The letter wrapped in `<em>` is the colored accent letter, and it is also the button that opens the "longer version" overlay. Put your own name in, and wrap whichever single letter you want to highlight in `<em>` (it carries the accent color and the click target).

### Tagline and page metadata
Search for `class="positioning"` for the one-line tagline under your name. Then update the page's metadata so links and browser tabs match: search the `<head>` for `<title>`, `og:title`, `og:description`, `og:url`, and the `twitter:` lines. Set `og:url` and the image URLs to your own domain.

### The four columns
Search for `class="meta"`. It holds four small blocks (in the original: Passion, Reach, Previously, Next). Rename the labels, rewrite the values, or add and remove blocks. They space themselves evenly, so you can have three or five.

### Connect buttons
Search for `class="connect"`. Replace the `href` values with your own links (LinkedIn, GitHub, a newsletter, a `mailto:` address). Add or drop buttons freely.

### The "longer version" overlay
Search for `id="about"`. This is the panel that opens when someone clicks the accent letter. It has a longer personal story and a list of past roles. Rewrite the paragraphs, and edit the role entries (each has a year, a title, and a short description).

### The two notes lower down
Search for `class="page-note"`. There are short paragraphs explaining the design and a short reflection. Rewrite them in your own words or remove them.

### Footer
Search for `<footer`. Swap the links and any text.

---

## Colors and phases

Search for `const palettes`. This object is the heart of the theming. Each phase (`dawn`, `midday`, `night`) has:

- `paper` — a light tint used by small UI surfaces (the toggle, the overlay).
- `bg` — the page background color for that phase.
- `ink` — the main text color.
- `muted` — secondary text (labels, captions).
- `accent` — links, button outlines, and the accent letter.
- `daubs` — an array of colors the brush strokes pull from.
- `dark` — set to `true` on dark phases. This flips the brush-stroke blend mode and nudges a couple of opacities so strokes stay visible on a dark background.

To retheme, change the hex values. The one rule to keep: light background needs dark `ink`, dark background needs light `ink`, or your text will be hard to read.

**Which phase shows when:** search for `const initial`. The hours are set there (dawn 5 to 11, midday 11 to 18, night otherwise). Change the ranges, or hard-code a single default if you do not want it to follow the clock.

**The cross-fade:** the smooth background change comes from `transition: background 1.4s ease` on the `body`. Change `1.4s` to taste. This works because the backgrounds are solid colors. Gradients do not cross-fade, they snap, which is why they are solid here.

---

## Cursor brush strokes

Search for `cursor brush strokes`. The strokes are drawn on a `<canvas>` that sits behind the content. New dabs are created on `pointermove` and fade out in the `frame` loop. Four knobs, all near each other:

- **Opacity:** `(p.dark ? 0.30 : 0.22)`. Higher is bolder.
- **Fade speed:** `d.life -= 0.009`. Smaller numbers make strokes last longer.
- **Trail length:** `daubs.length > 120`. The most strokes kept on screen at once.
- **Stroke size:** `(26 + Math.random() * 40)`. The radius range of each dab.

The strokes blend into the background using a CSS variable called `--blend`, set to `multiply` on light phases (so they read like translucent paint) and `screen` on night (so they glow). This is handled for you in `setLight`.

To remove the strokes entirely: delete the `<canvas id="water">` element near the top of `<body>`, and delete the whole `cursor brush strokes` block in the script.

---

## Music (read the license before you keep it)

Search for `const music`. Each phase maps to an audio file, a display name, and a volume. The audio is opt-in: the page is silent until the visitor clicks the Sound toggle, then it plays the piece for the current phase and crosses over when the phase changes.

To use your own audio: drop your files in `audio/`, and update the file names, display names, and volumes in the `music` object.

**The license matters.** The three pieces shipped with the original are from classicals.de under Creative Commons BY-NC 4.0 (recorded by Gregor Quendel). If you keep them, you must keep the credit line (it is in the "On This Design" note) and keep your site non-commercial. If that does not fit your plans, replace them with your own audio or royalty-free tracks, or remove music entirely: delete the files in `audio/`, the `music` object, the Sound button in the markup, and the audio block in the script.

---

## Fonts

The site uses Instrument Serif for display and Newsreader for body text, loaded from Google Fonts. Search the `<head>` for `fonts.googleapis.com` to find the link. To change fonts, swap that link and update the `--display` and `--body` variables near the top of the styles.

---

## Social card and favicon

`og-image.png` (1200x630) is what shows when someone shares your link. Replace it with your own, and point `og:image` and `twitter:image` in the `<head>` at your domain. Replace `favicon.svg` with your own mark and the browser tab will follow.

---

## Run it locally

Double-clicking `index.html` mostly works, but audio loads more reliably over a small local server. With Python installed:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

---

## Put it online

It is a static site, so any static host works: Vercel, Netlify, Cloudflare Pages, GitHub Pages, and others.

The general shape, whichever host you pick:

1. Push the folder to your own GitHub repository.
2. Import the repository into your host.
3. Set the framework preset to **Other** (or "static" / "no framework"). Leave the build command empty and the output directory at the root. There is nothing to build.
4. Deploy. You get a live URL in under a minute.
5. Connect your domain through the host's DNS instructions, then update `og:url` and the image URLs in `index.html` to match.

After that, most hosts redeploy automatically every time you push.

---

## A few good-to-knows

- **No dependencies.** The only things loaded from elsewhere are the two fonts and your audio files. Visitors need to be online for the fonts, which they will be.
- **Accessibility.** Keep your background-to-text contrast strong when you retheme. The brush strokes already turn themselves off for visitors who prefer reduced motion.
- **The code is yours to adapt.** Reuse and change it freely for your own site. The one binding constraint is the audio license above, which travels with those specific files, not with the code.

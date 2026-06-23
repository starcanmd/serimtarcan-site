# serimtarcan.com

A single-page personal site. Plain HTML, CSS, and JavaScript, no build step. The interactive background and the per-phase music run client-side. The three audio files live in `audio/`.

```
serimtarcan-site/
  index.html      the whole page
  favicon.svg
  og-image.png   social-share card (1200x630)
  audio/          three Impressionist recordings, one per phase
  .gitignore
```

---

## 1. Preview it locally

Double-clicking `index.html` works, but audio loads more reliably over a local server. With Python installed:

```bash
cd serimtarcan-site
python3 -m http.server 8000
```


---

## 2. Put it on GitHub

```bash
cd serimtarcan-site
git init
git add .
git commit -m "Initial site"
```

Create an empty repository on GitHub named `serimtarcan-site` (no README, since you have one), then:

```bash
git remote add origin https://github.com/starcanmd/serimtarcan-site.git
git branch -M main
git push -u origin main
```

---

## 3. Deploy on Vercel

1. Go to vercel.com and sign in with GitHub.
2. Click **Add New → Project**, import `serimtarcan-site`.
3. Framework Preset: **Other**. Leave Build Command empty and Output Directory as the root. There is nothing to build.
4. Click **Deploy**. In under a minute you get a live URL like `serimtarcan-site.vercel.app`.

Confirm the audio plays on that URL before connecting the domain.

CLI alternative: `npm i -g vercel`, then run `vercel` inside the folder and follow the prompts.

---

## 4. Buy and connect serimtarcan.com

The least painful route is buying the domain inside Vercel, because it wires up DNS for you.

**Option A, buy through Vercel (simplest):**
Project → **Settings → Domains** → type `serimtarcan.com` → **Buy**. Vercel registers it and points it at the project automatically. Add `www.serimtarcan.com` too and set it to redirect to the apex.

**Option B, buy elsewhere (Namecheap, Cloudflare, Porkbun, etc.):**
1. Buy `serimtarcan.com` at the registrar.
2. In Vercel: Project → **Settings → Domains** → add `serimtarcan.com` and `www.serimtarcan.com`.
3. Vercel shows you the exact DNS records to create. Use what it shows; the typical values are:
   - **A** record, host `@`, value `76.76.21.21`
   - **CNAME** record, host `www`, value `cname.vercel-dns.com`
4. Add those records in your registrar's DNS settings and save.

Either way, Vercel issues an HTTPS certificate automatically once DNS resolves. Propagation is usually minutes, occasionally up to a few hours.

---

## 5. Update the site later

Edit `index.html`, then:

```bash
git add .
git commit -m "Update copy"
git push
```

Vercel redeploys on every push to `main`. No other step.

---

## Notes

- **Fonts** load from Google Fonts at runtime, so visitors need to be online (they always are). Nothing to install.
- **Audio license:** the three pieces are from classicals.de under Creative Commons BY-NC 4.0. The "On This Design" section carries the required credit (the Gregor Quendel / classicals.de line). Keep the site non-commercial and that credit in place.
- **Social preview image:** `og-image.png` (1200×630) is included and already wired into the page's Open Graph and Twitter tags, so shared links render a card with your name and tagline.

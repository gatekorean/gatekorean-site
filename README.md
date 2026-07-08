# GateKorean — V1 static site

Approach C from the design doc: a branded static front door in Korean (default)
and English, handing off to Cal.com for booking/payment and Google Meet for
the actual lesson. No build step, no framework — plain HTML/CSS.

## Structure
- `/index.html` — redirects to `/ko/` or `/en/` based on browser language
- `/ko/index.html` — Korean homepage (default)
- `/en/index.html` — English homepage
- `/assets/style.css` — shared styles
- `netlify.toml` — Netlify build/redirect config

Japanese (`/ja`) is deferred to Phase 2.

## Before going live — fill these in
Search `ko/index.html`, `en/index.html`, `ko/books/index.html`, and
`en/books/index.html` for `EDITME`:
1. **Cal.com booking link** — replace `https://cal.com/gatekorean` with your real
   Cal.com event link, once your Cal.com account is set up and connected to Stripe.
2. **Teacher bio + photo** — replace the placeholder bio text.
3. **Formspree endpoint** — sign up at https://formspree.io, create a form, and
   replace `https://formspree.io/f/EDITME` (in the review form on both index
   pages) with your real form URL. Submissions are emailed to you — they do
   **not** appear on the site automatically.
4. **Reviews** — once you get a review by email that you'd like to feature,
   copy an `<article class="review">` block in the "What students say" /
   "수강생 후기" section and fill in the stars, comment, and name.
5. **Book Shelf entries** — in `ko/books/index.html` and `en/books/index.html`,
   copy an `<article class="book">` block per book. To use a real cover image,
   replace the placeholder `<div class="book-cover placeholder-cover">` with
   `<img class="book-cover" src="/assets/books/your-file.jpg" alt="..."
   width="100" height="150" loading="lazy">` and drop the image file in
   `assets/books/`.
6. **Contact email** — `hello@gatekorean.com` in the footer of both pages —
   update if you're using a different address.

## Deploy: GitHub + Netlify auto-deploy

### 1. Create the GitHub repo
Go to https://github.com/new while logged in as **gatekorean**, name it
(e.g. `gatekorean-site`), and create it **empty** (no README/license — this
folder already has one).

### 2. Push this folder
```
cd ~/Documents/monique-language-school
git remote add origin https://github.com/gatekorean/gatekorean-site.git
git branch -M main
git add .
git commit -m "V1 static site: ko/en front door"
git push -u origin main
```

### 3. Connect Netlify
- Log into https://app.netlify.com
- **Add new site → Import an existing project → Deploy with GitHub**
- Authorize Netlify for the `gatekorean` GitHub account, pick the
  `gatekorean-site` repo
- Build settings: leave build command empty, publish directory `.`
  (already set in `netlify.toml`, Netlify should detect it automatically)
- Deploy — every push to `main` will now auto-deploy

### 4. Connect gatekorean.com (registrar: Gabia)
- In Netlify: **Site settings → Domain management → Add a domain** → enter
  `gatekorean.com`
- Netlify will show you DNS records to add. Two options:
  - **Easiest:** Netlify gives you nameservers (e.g. `dns1.p0X.nsone.net`) —
    go to Gabia's domain management panel and change gatekorean.com's
    nameservers to Netlify's. Netlify then manages all DNS.
  - **Alternative (keep DNS at Gabia):** Netlify gives you an apex A record
    (usually `75.2.60.5`) and a `www` CNAME (`<your-site>.netlify.app`) —
    add those as records inside Gabia's DNS panel instead of switching
    nameservers.
- DNS propagation can take anywhere from a few minutes to ~24 hours.
- Once it resolves, go back to Netlify's domain settings and enable
  **HTTPS** (Netlify provisions a free Let's Encrypt certificate
  automatically once DNS is verified).

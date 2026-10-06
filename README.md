# Moonshot · Jalapeño landing pages

Static pages for moonshot.computer. Each folder is the URL it serves; every page is one self-contained `index.html`
(CSS and JS inline, no build step) and the pages share `/img` and `/vid`. On the live site these files sit under
`/landing-pages-oct2/`, so keep the folder layout as it is here.

## What's live

| Folder | URL | Page |
|---|---|---|
| `/` (`index.html`) | moonshot.computer/**a** | "Handled before you ask." The main ad page. |
| `f/` | moonshot.computer/**f** | Ad lander for the "Handled before you ask." ads, lp-0x structure, no persona. |
| `e/` | moonshot.computer/**e** | "Laugh with it. Argue with it. Build with it." Second universal page. **Not live yet: /e shows the contact page (see Changes).** |
| `lp-01/` | moonshot.computer/**b** | Adults with ADHD |
| `lp-02/` | moonshot.computer/**c** | Working dads |
| `lp-03/` | moonshot.computer/**d** | Moms |
| `contact/` | moonshot.computer/contact | Contact page. **Live /contact still shows the old page (see Changes).** |

The `lp-0x` folder names don't match their live paths (/b, /c, /d); keep the mapping above.

## Changes (newest first)

Each change ships only the files listed. Don't redeploy anything else.

- **6 Oct, FIX /e and /contact (they're swapped on the live site).** Checked 6 Oct: moonshot.computer/e shows the new
  contact page ("Questions? Write to us."), and /contact shows the old one. Serve `e/index.html` (the "Laugh with it."
  page, with `vid/e-hero.mp4`) at /e, and `contact/index.html` at /contact.

- **6 Oct, /a hero video plays on iPhone. UPLOAD 2 FILES: `index.html` and `vid/uni-hero.mp4`.**
  The hero showed a still frame on iPhones and inside the Facebook and Instagram in-app browsers. Two fixes:
  `vid/uni-hero.mp4` is now the 720p version (the same clip /e uses, which plays on iPhone; the old file was
  1600x900), and `index.html` retries playback when the clip is ready, when the tab comes back, and on the first
  touch or scroll. Nothing else on /a changed. Phones in Low Power Mode still block autoplay on every site and show
  the poster until touched.
- **5 Oct, new /f: `f/index.html`** plus `img/speaks-hero.webp`, `img/speaks-close.webp`,
  `vid/speaks-step1.mp4/.jpg`, `vid/speaks-step3.mp4/.jpg`.
- **5 Oct, new /e: `e/index.html`** plus `vid/e-hero.mp4`.
- **5 Oct, /a:** a subline change was made and reverted the same day. /a's copy is unchanged.

## Checkout and tracking

Every page's Pre-order button must go to the live Stripe checkout, and every page needs the live Meta pixel and
conversion events (Purchase and InitiateCheckout), the same as /a. The ad sets optimize for Purchase.

## Editing

- `/a` (root `index.html`) is edited directly in this repo. It is not generated.
- `/e`, `/f`, the `lp-0x` pages and `/contact` are generated from `coldoo/creative-strategy`
  (`work/LP-2026-10-02/site/`) and exported here with `export_repo.py`. The export never touches the root page or
  `vid/uni-hero.mp4`.

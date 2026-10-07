# Moonshot · Jalapeño landing pages

Static pages for moonshot.computer. Each folder is the URL it serves; every page is one self-contained `index.html`
(CSS and JS inline, no build step) and the pages share `/img` and `/vid`. On the live site these files sit under
`/landing-pages-oct2/`, so keep the folder layout as it is here.

## To upload now (7 Oct)

Two changes, then one fix. Ship only the files listed; don't redeploy anything else.

### 1. /a: new section order and the hero video fix

**Upload 2 files: `index.html` and `vid/uni-hero.mp4`.**

- **Section order.** The top is unchanged through "Thought you'd want to know." Then: your apps, how it works
  (3 steps), privacy, offer, specs, a new MCP section (from /e), the comparison and the FAQ. The hero's "Privacy built
  on iPhone's Secure Enclave..." line now jumps down to the privacy section. No copy changed.
- **Hero video on iPhone.** The hero showed a still frame on iPhones and inside the Facebook and Instagram in-app
  browsers. `vid/uni-hero.mp4` is now the 720p version (the same clip /e uses, which plays on iPhone; the old file was
  1600x900), and `index.html` retries playback when the clip is ready, when the tab comes back, and on the first touch
  or scroll. Phones in Low Power Mode still block autoplay on every site and show the poster until touched.

### 2. New: three listicle pages (pre-landers for the ads)

Short articles that sit between the ad and /a: they explain Moonshot first, then send people to /a to order. They're
`noindex`, so they stay out of search. All three live in one `listicle/` folder:

| Id | Folder and URL | Page |
|---|---|---|
| LST-001 | `listicle/never-set-a-reminder/` → moonshot.computer/listicle/never-set-a-reminder | "7 reasons why you should never set a reminder again" |
| LST-002 | `listicle/phone-cant/` → moonshot.computer/listicle/phone-cant | "The earpiece that speaks up before you ask: 5 things it does that your phone can't" |
| LST-003 | `listicle/busy-dads/` → moonshot.computer/listicle/busy-dads | "5 reasons why busy dads reach for Moonshot" |

**Upload:** the three `listicle/<slug>/index.html` files, plus these 14 new images in `img/`:
`lst-bleachers.webp`, `lst-blocks.webp`, `lst-car.webp`, `lst-couch-phone.webp`, `lst-dad2.webp`,
`lst-dinner.webp`, `lst-groceries.webp`, `lst-hand-palm.webp`, `lst-hand.webp`, `lst-office.webp`,
`lst-pickup.webp`, `lst-talk.webp`, `lst-talking.webp`, `lst-walk.webp`.
They also use images already live (`speaks-hero.webp`, `speaks-close.webp`, `p-orange.webp`, `p-black.webp`,
`p-blue.webp`, `msmark.png`). Keep these exact URLs; the ads will point at them.

**Tracking (done separately, by whoever runs the pixel).** The files here carry no Meta pixel. Once the pages are
live, the same Meta pixel as /a (id 1897318591235781, PageView) goes on all three listicle URLs, once each, so every
visit counts once. Every button already goes to `https://moonshot.computer/a` and carries the visitor's `fbclid` and
`utm_*` along, so a purchase on /a still traces back to the ad.

### 3. Fix: /e and /contact are swapped on the live site

Checked 6 Oct: moonshot.computer/e shows the new contact page ("Questions? Write to us."), and /contact shows the old
one. Serve `e/index.html` (the "Laugh with it." page, with `vid/e-hero.mp4`) at /e, and `contact/index.html` at
/contact.

## What's live

| Folder | URL | Page |
|---|---|---|
| `/` (`index.html`) | moonshot.computer/**a** | "Handled before you ask." The main ad page. |
| `f/` | moonshot.computer/**f** | Ad lander for the "Handled before you ask." ads, lp-0x structure, no persona. |
| `e/` | moonshot.computer/**e** | "Laugh with it. Argue with it. Build with it." **Not live yet: /e shows the contact page (fix 3).** |
| `lp-01/` | moonshot.computer/**b** | Adults with ADHD |
| `lp-02/` | moonshot.computer/**c** | Working dads |
| `lp-03/` | moonshot.computer/**d** | Moms |
| `contact/` | moonshot.computer/contact | Contact page. **Live /contact still shows the old page (fix 3).** |
| `listicle/*/` | not live yet | The listicles above (change 2). |

The `lp-0x` folder names don't match their live paths (/b, /c, /d); keep the mapping above.

## Earlier changes

- **5 Oct, new /f: `f/index.html`** plus `img/speaks-hero.webp`, `img/speaks-close.webp`,
  `vid/speaks-step1.mp4/.jpg`, `vid/speaks-step3.mp4/.jpg`.
- **5 Oct, new /e: `e/index.html`** plus `vid/e-hero.mp4`.
- **5 Oct, /a:** a subline change was made and reverted the same day.

## Checkout and tracking

Every page's Pre-order button must go to the live Stripe checkout, and every page needs the live Meta pixel and
conversion events (Purchase and InitiateCheckout), the same as /a. The ad sets optimize for Purchase. The listicles
have no checkout of their own; they need the same pixel as /a (see change 2).

## Editing

- `/a` (root `index.html`) is edited directly in this repo. It is not generated.
- The `listicle/` pages are generated from `coldoo/creative-strategy` (`work/LP-2026-10-06-listicles/build_pages.py`,
  from each page's `copy.md`).
- `/e`, `/f`, the `lp-0x` pages and `/contact` are generated from `coldoo/creative-strategy`
  (`work/LP-2026-10-02/site/`) and exported here with `export_repo.py`. The export never touches the root page or
  `vid/uni-hero.mp4`.

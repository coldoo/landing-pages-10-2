# Moonshot · Jalapeño landing pages

Static pages for moonshot.computer. Each folder is the URL it serves; every page is one self-contained `index.html`
(CSS and JS inline, no build step) and the pages share `/img` and `/vid`. On the live site these files sit under
`/landing-pages-oct2/`, so keep the folder layout as it is here.

## Upload first (19 Oct): the free year is gone, a subscription FAQ is added

The founders dropped the free year on 14 Oct, but the live pages still promise it. **Upload these 6 files:**
`index.html` (/a), `f/index.html` (/f), `e/index.html` (/e), `lp-01/index.html` (/b), `lp-02/index.html` (/c),
`lp-03/index.html` (/d). No new images.

- The offer list's "1 free year of Jalapeño" is now "Founding batch beta access".
- The top bar and the sticky price bar say "Fully refundable" instead of "First year included"
  (the long bar: "$99 (retail $150), fully refundable before it ships.").
- Every FAQ has a new entry, "Is there a subscription?", with no price and no date.

These files also carry the new offer section (10 Oct, below), which waits on the checkout change. If the checkout isn't
ready, make the same three text changes by hand on the live pages instead.

## To upload now (15 Oct): new listicle LST-005 at /lst-005

A new pre-lander for the ads: "We asked 50+ AI wearable owners what's still missing. Here's what they told us."
It is `noindex`, so it stays out of search, and every button goes to `https://moonshot.computer/a` (carrying the
visitor's `fbclid` and `utm_*` along).

**Upload:** `lst-005/index.html`, served at **moonshot.computer/lst-005**, plus 2 new images in `img/`:
`lst-transcript.webp` and `lst-contractor.webp`. Its other images are already live (`dlife3.webp`, `lst-office.webp`,
`lst-hand-palm.webp`, `speaks-close.webp`, `speaks-hero.webp`, `p-orange.webp`, `msmark.png`).

**Tracking (done separately, by whoever runs the pixel):** the same Meta pixel as /a (PageView) goes on /lst-005,
once, like the other listicles.

## To upload first (11 Oct): /a hero becomes a still image

The /a hero video shows blank in the Facebook and Instagram in-app browsers, which is where ad traffic lands. It is now
a still photo (the clip's last frame: she wears the earpiece and smiles).

**Upload 1 new image: `img/a-hero.webp`.** Then, on the live /a, replace the hero's `<video class="uvid bv" ...></video>`
tag (inside `<div class="ubg">`) with this one line, and change nothing else:

```html
<img class="uvid" src="/landing-pages-oct2/img/a-hero.webp" width="1280" height="720" fetchpriority="high" alt="A woman wearing the Jalapeño earpiece behind her ear, smiling">
```

`index.html` in this repo already has it, but it also carries the new offer section below, which waits on the
checkout change. So patch the live /a by hand now, and the full `index.html` goes up later with the offer section.

## To upload now (10 Oct): the new offer section

**Upload 6 files:** `index.html` (/a), `f/index.html` (/f), `e/index.html` (/e), `lp-01/index.html` (/b),
`lp-02/index.html` (/c), `lp-03/index.html` (/d). No new images; the color photos (`img/p-orange.webp`,
`p-blue.webp`, `p-black.webp`) are already live.

What changed, the same on all six pages: the offer box now reads Jalapeño, then "Everything included in your order",
then two cards side by side, **Single** $99 (was $150, selected by default) and **Couple Set** $179 (was $300), then
one color row per earpiece (the second row shows only for the Couple Set), then the Pre-order button. The quantity
stepper and the old price line are gone.

**Checkout change needed, in the same upload (the site's checkout code).** Single works as it does today. For the
Couple Set, the Pre-order button sends `data-qty="2"`, `data-color` (earpiece 1) and `data-color-2` (earpiece 2).
The checkout must:

1. Charge **$179** with Stripe price `price_1UPBLSCz1pX14wUGfCxSKnQV` (lookup key `jalapeno_couple_set_179`) at
   quantity 1, not the $99 price at quantity 2.
2. Save **both colors** on the order (today only `data-color` is sent to `/api/preorder-checkout`).

Don't put the new pages live until a Couple Set test checkout shows $179 and both colors.

## Still to upload, if not done yet (7 Oct)

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
| `lst-005/` | moonshot.computer/**lst-005** | LST-005, "We asked 50+ AI wearable owners..." (15 Oct). |

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
- The offer section on all six pages is patched in after export by
  `work/LP-2026-10-02/site/offer_v2.py` (creative-strategy); re-run it on each page after any re-export.

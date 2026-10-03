# Moonshot · Jalapeño landing pages (2 Oct 2026)

## Preview first

**See all five pages working before you build anything:** https://claude.ai/artifact/G33mDVHjQBA8JJq2Dfocg9

Each page there is labeled with the URL it goes live at. Open any of them on desktop or on a phone. The preview shows
the same pages as this repo; only the links between them differ (in the preview the files sit side by side, here each
page is in its URL folder).

## The pages

Static pages, ready to host. Each folder is the URL it serves. Every page is one self-contained `index.html`
(CSS and JS inline, no build step); the pages share `/img` and `/vid`.

| Folder | URL | What it is |
|---|---|---|
| `/` (`index.html`) | **moonshot.computer** | The universal homepage. Replaces the current homepage. |
| `lp-01/` | **moonshot.computer/lp-01** | Ad landing page for adults with ADHD |
| `lp-02/` | **moonshot.computer/lp-02** | Ad landing page for working dads |
| `lp-03/` | **moonshot.computer/lp-03** | Ad landing page for moms |
| `contact/` | **moonshot.computer/contact** | A new, friendlier contact page (see below) |
| `img/`, `vid/` | `/img/...`, `/vid/...` | Shared images (WebP) and videos (MP4 + poster JPGs) |

All asset paths are root paths (`/img/...`, `/vid/...`), so the folders must stay at the site root as laid out here.
Any static host works (Vercel, Netlify, Cloudflare Pages, S3, Webflow custom code): a folder's `index.html` is served at
`/lp-01` etc. If your host wants flat files instead, `lp-01/index.html` can become `lp-01.html`, as long as `/img` and
`/vid` stay at the root.

## What's on each page

**Homepage (`/`)** is built like a brand page:
1. A banner video: a hand turns the earpiece, she puts it on, and she smiles.
2. The "Inspired by *Her*" strip, which links to the film.
3. Meet Jalapeño, with finished tasks scrolling behind the product.
4. "Thought you'd want to know.", five moments in a swipeable carousel.
5. Specs, then privacy (with the Secure Enclave and iCloud Keychain cards and the pause button).
6. The offer (gallery, color, quantity, what's included, the checkout button).
7. How it works, in three steps with clips.
8. The apps grid.
9. A comparison against Omi, Plaud and Pocket.
10. The FAQ (copied from the live site) and a small "Have questions? Contact us" strip.

**The three landers (`/lp-01`, `/lp-02`, `/lp-03`)** follow the same direct-response structure, each with its own
headline, bullets, people, problem, step clips, comparison and photos:

offer bar → hero with bullets and bubbles → YC strip → problem → Meet Jalapeño → three steps with clips → apps →
offer → specs → privacy → comparison → FAQ → founder story → contact strip.

They also have a slim floating bar at the bottom with the price and a Pre-order button. It hides while the offer is
on screen.

**Contact (`/contact`) replaces the current contact page.** It is served at the same address,
moonshot.computer/contact, so deploying this folder replaces the page that's there now. It keeps everything the
current page says (one address for everything, what to include for support, privacy and deletion requests, press and
partnerships, and the San Francisco business location), written as friendlier plain text with jump links at the top.
It names one address, privacy@moonshot.computer, and has no phone number. Every page's "Contact" link points to
`/contact`.

## Reminders before launch

Checkout and tracking already exist on the live site. These pages are new HTML, so carry them over:

1. **Checkout.** The checkout button on each page is `<a id="rsv-btn" href="#offer">` (once each on `index.html`,
   `lp-01`, `lp-02` and `lp-03`). Point it to the existing Stripe checkout, the same one the live homepage uses. The
   button also carries the shopper's choice as `data-color` and `data-qty`, if your checkout uses them.
2. **Tracking.** Add the existing Meta pixel, Google Ads tag and conversion events to all four pages, the same setup
   as the live site. Purchase must keep firing, because the ad sets optimize for it.
3. **"190 units left".** This is fixed text in the offer (`<em>190 units left</em>`). Either connect it to the live
   count from Stripe, or remove it.
4. **The live homepage.** `/` replaces the current homepage. Keep anything the live page does that this one doesn't,
   such as the $10 friend-referral link.

"Privacy" links to moonshot.computer/privacy, and "About" to moonshot.computer/about. Note that /about still describes
the old iPhone-app beta.

## Videos and images

- Videos are MP4s with JPG posters. A small script on each page downloads a clip whole and plays it from memory,
  so clips also play inside embedded previews and on hosts that don't support byte-range requests (iOS needs those
  for a normal MP4). On a normal host they play either way. Serve `.mp4` as `video/mp4`.
- Images are compressed WebP (the product shots keep transparent backgrounds). Fonts load from Google Fonts
  (Hanken Grotesk, Newsreader).
- Layout is tested on desktop and on phones down to 310px wide (the iPhone in-app viewer). There is no sideways scroll
  on phones.

## Editing

The pages are generated. The source (copy files, builders, CSS layers) lives in `coldoo/creative-strategy` under
`work/LP-2026-10-02/site/`. After a change, rebuild and re-export into this repo:

```
python -X utf8 work/LP-2026-10-02/site/build_v12.py
python -X utf8 work/LP-2026-10-02/site/build_contact.py
python -X utf8 work/LP-2026-10-02/site/build_universal.py
python -X utf8 work/LP-2026-10-02/site/export_repo.py C:/landing-pages-10-2
```

Small text fixes can also be made directly in these HTML files. The next export overwrites them, so make lasting
changes in the source.

Optional: the three `/lp-0x` pages are ad landers. If you don't want them in search results, add
`<meta name="robots" content="noindex">` to their `<head>`.

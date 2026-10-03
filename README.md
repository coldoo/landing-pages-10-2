# Moonshot · Jalapeño landing pages (2 Oct 2026)

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

**Contact (`/contact`)** is new: plain text like the current contact page, but friendlier. It has jump links for
pre-orders, using Jalapeño, privacy and deletion, and press. It names one address, privacy@moonshot.computer, and has
no phone number. Every page's "Contact" link points to `/contact`. To keep the existing contact page instead, change
those links.

## To wire up before launch

1. **Checkout.** Every Pre-order button scrolls to the offer section. The checkout button there is
   `<a id="rsv-btn" href="#offer">` on every page. Replace `#offer` with your Stripe Checkout or Payment Link.
   The page keeps the shopper's choice on that button as `data-color` (orange / blue / black) and `data-qty` (1-10),
   so your checkout can pass the color and quantity along (as Checkout Session metadata or a quantity, for example).
2. **Pixel and conversion events.** These pages fire no tracking yet. Add the Meta pixel (and the Google Ads tag) to each
   `<head>`, then fire:
   - `ViewContent` on load;
   - `AddToCart` on a color or quantity change;
   - `InitiateCheckout` on the `#rsv-btn` click;
   - `Purchase` from the Stripe success page, ideally also server-side through the Conversions API, deduplicated
     by event id.

   Ad sets optimize for Purchase, so that event has to work.
3. **Units left.** "190 units left" in the offer is static text. Wire it to the real count, or remove it.
4. **Links.** "Privacy" goes to moonshot.computer/privacy, and "About" to moonshot.computer/about. Note that /about
   still describes the old iPhone-app beta.

## Still to confirm with Dylan

- The offer terms shown on the pages: $99 (retail $150), "1 free year of Jalapeño", "1-year warranty", "Priority
  shipping for the first batch", "First access to everything we build", "Full refund anytime before shipping" and
  "Ships December 2026".
- The founder story on the three landers is Dylan's family story, signed by all three founders. It needs his sign-off.
- The comparison marks were researched from each rival's own site on 2 Oct 2026; the sources are listed in the build
  repo. A rival feature launched within the last month counts as ✗ (Plaud's agent). Re-check before launch.

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

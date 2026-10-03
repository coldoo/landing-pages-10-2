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

**Contact (`/contact`) replaces the current contact page.** It is served at the same address,
moonshot.computer/contact, so deploying this folder replaces the page that's there now. It keeps everything the
current page says (one address for everything, what to include for support, privacy and deletion requests, press and
partnerships, and the San Francisco business location), written as friendlier plain text with jump links at the top.
It names one address, privacy@moonshot.computer, and has no phone number. Every page's "Contact" link points to
`/contact`.

## To wire up before launch

The pages work as soon as they're hosted, but they can't take orders or report sales until steps 1 and 2 are done.
Do them before any ad traffic points here.

### 1. Connect checkout (required)

Every "Pre-order" button on a page scrolls to the offer section. The button that should open checkout is the big
"Pre-order Jalapeño now" button in the offer:

```html
<a class="btn" id="rsv-btn" href="#offer">Pre-order Jalapeño now</a>
```

It appears once on each of the four pages (`index.html`, `lp-01`, `lp-02`, `lp-03`). Replace `#offer` with the Stripe
checkout link. The live homepage already has a working Stripe pre-order, so reuse that link or product.

The page keeps the shopper's choices on that button, updated as they click:
- `data-color` is `orange`, `blue` or `black`;
- `data-qty` is 1 to 10, and the price shown updates to $99 × qty.

To send the shopper to checkout with those choices, intercept the click. For example, with a Checkout Session created
by your server:

```html
<script>
document.getElementById('rsv-btn').addEventListener('click', async function (e) {
  e.preventDefault();
  const color = this.dataset.color || 'orange';
  const qty = Number(this.dataset.qty || 1);
  // your endpoint creates a Stripe Checkout Session (price = $99, quantity = qty, metadata.color = color)
  // and returns its URL
  const r = await fetch('/api/checkout', {method: 'POST', headers: {'Content-Type': 'application/json'},
                                          body: JSON.stringify({color, qty})});
  const {url} = await r.json();
  location.href = url;
});
</script>
```

With Stripe Payment Links instead (no server), make one link per color and pick it by `data-color`. Turn on
"adjustable quantity" in the link to let the shopper change quantity in Stripe.

### 2. Add tracking (required for ads)

These pages fire no tracking yet. The live homepage loads the Meta pixel and a Google Ads tag; replacing the homepage
removes them, so add them back to the `<head>` of all four pages (`index.html`, `lp-01`, `lp-02`, `lp-03`). Use the
same pixel ID and Ads tag as the live site.

Then fire these events. The ad sets optimize for **Purchase**, so that one has to work.

| Event | When | How |
|---|---|---|
| `PageView` | page load | included in the pixel base code |
| `ViewContent` | page load | snippet below |
| `AddToCart` | shopper changes color or quantity | snippet below |
| `InitiateCheckout` | click on `#rsv-btn` | snippet below |
| `Purchase` | the Stripe success page, after payment | on the success page, plus the Conversions API from a Stripe webhook |

Paste this before `</body>` on the four pages, after the pixel base code:

```html
<script>
(function () {
  if (!window.fbq) return;
  var btn = document.getElementById('rsv-btn');
  var value = function () { return 99 * Number(btn.dataset.qty || 1); };
  fbq('track', 'ViewContent', {content_name: 'Jalapeño', value: 99, currency: 'USD'});
  document.querySelectorAll('.colors button, #q-plus, #q-minus').forEach(function (el) {
    el.addEventListener('click', function () {
      fbq('track', 'AddToCart', {content_ids: [btn.dataset.color || 'orange'], value: value(), currency: 'USD'});
    });
  });
  btn.addEventListener('click', function () {
    fbq('track', 'InitiateCheckout', {content_ids: [btn.dataset.color || 'orange'],
                                      num_items: Number(btn.dataset.qty || 1), value: value(), currency: 'USD'});
  });
})();
</script>
```

**Purchase** belongs on the page Stripe returns to after payment (the Checkout `success_url`):
`fbq('track', 'Purchase', {value: <amount>, currency: 'USD'}, {eventID: '<checkout session id>'})`.

Also send the same event server-side through Meta's Conversions API from a Stripe `checkout.session.completed`
webhook, with the same event ID so Meta counts it once. This catches buyers whose browsers block the pixel.

Pass the ad's UTM tags and `fbclid` into the Checkout Session metadata (never in the URL) to tie each order to its ad.
Mirror Purchase to the Google Ads tag as a conversion.

### 3. Units left: make it live, or remove it (optional)

The offer shows "Founding batch · **190 units left**". The number is fixed text in each page:

```html
<span class="urg">Founding batch · <em>190 units left</em></span>
```

To make it real, count paid pre-orders in Stripe and show the batch size minus the units sold:

1. Add a small server endpoint, for example `/api/units-left`, that sums the quantity of paid Checkout Sessions for the
   Jalapeño price. Use Stripe's `checkout.sessions.list` with `status: 'complete'`, or keep a running total that the
   `checkout.session.completed` webhook updates. It returns `BATCH_SIZE - sold` (set `BATCH_SIZE` to the real founding
   batch size; the pages assume 200). Cache the result for a minute or so, so page loads don't hit Stripe each time.
2. Paste this before `</body>` on the four pages:

```html
<script>
fetch('/api/units-left').then(function (r) { return r.json(); }).then(function (d) {
  var el = document.querySelector('.urg em');
  if (el && typeof d.left === 'number') el.textContent = d.left > 0 ? d.left + ' units left' : 'Sold out';
}).catch(function () {});
</script>
```

If the endpoint fails, the page keeps showing the fixed text. If the count won't be wired up, remove the
`<em>190 units left</em>` part (or the whole `urg` line) so the page never shows a number that isn't true.

### 4. Check what the current homepage does before replacing it

`/` replaces the current moonshot.computer homepage. Before swapping it, keep anything the live page handles that
this one doesn't:
- the Stripe checkout link (step 1);
- the pixel and Google Ads tag (step 2);
- the $10 friend-referral link and card;
- any other scripts the live page loads.

Other links: "Privacy" goes to moonshot.computer/privacy, and "About" to moonshot.computer/about. Note that /about still
describes the old iPhone-app beta.

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

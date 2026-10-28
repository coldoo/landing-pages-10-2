# /reserve: what to wire up before it goes live

Two pages: `reserve/index.html` (moonshot.computer/reserve) and `reserve/thanks/index.html`
(moonshot.computer/reserve/thanks). Both are plain static HTML with images in `/img/moon-*` and `/img/samantha.webp`.
Everything below is a find-and-replace or a setting. Nothing else on the pages needs to change.

## 1. Stripe: the $1 reservation

- Create a $1 product ("Moon founding reservation"), a one-time payment. Turn on Apple Pay and Google Pay.
- Make a Payment Link for it. After payment, redirect to `https://moonshot.computer/reserve/thanks`.
- Collect email at checkout (Stripe does by default) and phone if you can. That list is who we text on launch day.
- **`reserve/index.html`**: replace `[STRIPE_$1_CHECKOUT]` (1 place, the "Reserve the Moon for $1" button) with the
  Payment Link URL. Every other Reserve button on the page scrolls to that one.
- Refunds: the page promises "Refund in one tap" and "refundable anytime". Whatever the refund route is (reply to the
  receipt email, a form, Stripe's customer portal), it has to work before launch.

## 2. Pixel

Both pages need the site's Meta pixel base code (same pixel as the rest of moonshot.computer: PageView).
On **`reserve/thanks/index.html`** only, also fire the purchase, once, after the base code:

```html
<script>fbq('track', 'Purchase', {value: 1.00, currency: 'USD', content_name: 'Moon reservation'});</script>
```

The ads will optimize for this event, so test it with Meta's Test Events tool before anything goes live.

## 3. Dates

| File | Find | Replace with | Places |
|---|---|---|---|
| `reserve/index.html` | `Nov 18` | the Kickstarter launch date | 2 (offer box, FAQ) |
| `reserve/index.html` | `spring 2027` | the ship window | 2 (offer box, FAQ) |
| `reserve/thanks/index.html` | `Tue, Nov 18` | launch day and date | 1 |
| `reserve/thanks/index.html` | `9 AM PT` | launch time | 1 |
| `reserve/thanks/index.html` | `20261118T170000Z` and `20261118T173000Z` | launch start and end in UTC (9 AM PT in November = 17:00Z) | 1 each, in the calendar script |

## 4. Launch text and email signup (thank-you page)

- `[SMS_ENDPOINT]` is the form's `action`. The form posts one field, `name="phone"`. Point it at your SMS tool
  (Klaviyo, Postscript, Attentive), or swap the whole `<form id="sms">` block for the tool's own embed.
  Until it's set, the form only shows "You'll get a text at launch" and sends nothing.
- `[VIP_GROUP_LINK]` is the "Join on WhatsApp" button. Paste a WhatsApp community invite link, or delete that whole
  card (`<div class="card">` with "The VIP group") if you don't want a group.
- The share box already holds `https://moonshot.computer/reserve`. Change it only if the URL changes.

## 5. Confirm before launch (the page promises these)

- **Starlight box "free".** The page lists the box as free with the reservation.
- **6 months of Samantha free.** No monthly price is shown anywhere, on purpose.
- **First Light engraving, numbered, first 300 only.** One gallery photo shows "FIRST LIGHT 001" on a satin back.
- **$499 on Kickstarter, no early-bird price.**
- If any of these change, tell James and he'll update the page and the ads.

## 6. Elsewhere on the site

- moonshot.computer/moon still says "Pre-order · $99". Anyone who goes from /reserve to /moon sees a different
  price. Change /moon (or point its button at /reserve) before the ads run.
- Any real photo or video of a working prototype, even rough. Every image on /reserve is a render or AI photo.

## 7. Send back to James when done

1. The live /reserve URL, with the Stripe link working (a real $1 test, then refund it)
2. Confirmation that Purchase fires on /reserve/thanks in Test Events
3. The launch date and ship window you used

He uploads the 6 statics PAUSED into their own ad set, optimized for that Purchase event, and turns them on only once
you've both said go.

# Importing the Opt-In Page into GoHighLevel (Funnels)

This folder is a repackaging of `../index.html` into the shape GHL's
**Funnels** builder expects: page-level CSS/fonts in one field, and one
**Custom HTML** element per row. Copy and layout are unchanged from the
approved design — this is packaging only.

## 1. Paste the CSS

GHL steps typically expose two different fields for this — use whichever
one your funnel step actually has:

- **Settings → Tracking Code → Head** (accepts full HTML): paste the
  entire contents of **`head-code.html`** (Google Fonts `<link>` tags +
  the `<style>` block, all in one).
- **Settings → Custom CSS** (raw CSS only, no `<style>`/`<link>` tags):
  paste **`custom-css.css`** instead. If your step *only* has this field
  and no separate head/tracking field for the fonts, uncomment the
  `@import` line at the top of `custom-css.css`.

That CSS is scoped under a single `.fsp-page` wrapper class on purpose —
every section fragment below opens with `<div class="fsp-page">` for exactly
this reason. This keeps the reset/base styles from reaching anything else
GHL renders on this page (cookie banners, chat widgets, other native
elements). **Don't strip that wrapper div when you paste sections in.**

If this step's Tracking Code field ever rejects or strips `<style>` tags,
fall back to pasting the same block as a Custom HTML element in the very
first row of the page, above the topbar row.

## 2. Add five full-width rows, in this order

For each row: add a row (full width, no GHL background/padding needed — the
CSS handles all of that), drop in a **Custom HTML** element, and paste the
matching file:

| Row | File | Contains |
|-----|------|----------|
| 1 | `section-1-topbar.html` | Wordmark bar |
| 2 | `section-2-hero.html` | Hero copy + the opt-in form (one block — the two-column layout is CSS grid inside the fragment, so it doesn't need separate GHL columns) |
| 3 | `section-3-trustbar.html` | Agent photo/license + carrier logos |
| 4 | `section-4-belief.html` | "The Coverage Illusion" stat section |
| 5 | `section-5-footer.html` | Footer + compliance/legal copy |

**Common gotcha:** if a section shows up centered with gray margins on
both sides instead of running edge-to-edge, that's GHL's own Section
width setting, not the CSS. Click the Section (not the element) → find
**Content Width** and set it to **Full Width / Stretch** (not "Boxed"),
and set the Section's left/right padding to **0** (GHL's default padding
will otherwise double up with the padding already built into the CSS).
This is a per-section setting — repeat it for all five rows.

## 3. Wire the form

Two ways to do this, depending on what you're using downstream:

- **Plain webhook** — if you're not relying on GHL's Meta Conversions API
  integration, just point the form at your GHL inbound webhook URL:
  ```html
  <form class="form-card" id="optin" method="post" action="YOUR_WEBHOOK_URL_HERE">
  ```
  Field names sent are `full_name`, `email`, `phone`, `consent` — plain
  names chosen so they map easily to most webhook/workflow setups.
- **Relay into a hidden native GHL form (for Meta CAPI)** — if you're
  sending qualified leads back to Facebook via GHL's Conversions API
  integration, a raw webhook won't carry the browser-side data (`_fbp`/
  `_fbc` cookies, IP, user agent) that CAPI matching needs — only a native
  GHL Form capture does. See step 6a below for the full setup; leave
  `action="#"` on this form in that case, since the hidden form submits
  instead.

## 4. No JavaScript is needed

This page has no interactive behavior beyond anchor scrolling (the belief
section's CTA jumps to `#optin`) — handled by the `scroll-behavior: smooth`
rule already in `head-code.html`. There's nothing to add to a Tracking Code
(Footer)/JS field for this page as built. If you later add client-side
validation for the webhook, that's the place it would go.

## 5. Publish the Privacy Policy and Terms of Use as two more GHL pages

`../privacy-policy.html` and `../terms-of-use.html` are generic legal-page
templates (same look and feel as the opt-in page). **These are boilerplate,
not attorney-reviewed** — insurance advertising is state-regulated, so have
compliance/legal review them before running paid traffic, and fill in the
`[DATE — set on publish]` placeholder in each.

Each is a full standalone page (its own `<head>`/`<style>`, no `.fsp-page`
scoping needed) — publish each as its own funnel step/page in GHL, then
update the two links in `section-5-footer.html` and the "Privacy Policy"
link in `section-2-hero.html`'s consent line from the current relative
paths (`privacy-policy.html`, `terms-of-use.html`) to whatever URLs GHL
gives those published pages.

## 6. Optional: scroll-triggered exit popup

`popup-exit-optin.html` is a second, smaller opt-in styled to match the
page (cream card, gold border, same trust line), meant to catch visitors
who scroll to the bottom without filling out the hero form. Same fields,
same TCPA consent language — copy leans slightly more urgent ("I'll check
it later" is how the gap stays open") since it's a last-chance moment, but
still no fabricated scarcity, per the Forbidden Persuasion guardrail.

Two versions, depending on what your GHL page gives you:

- **`popup-exit-optin.html`** — use this if you're placing it as a plain
  Custom HTML element with no native popup shell around it. It's fully
  self-contained: its own fixed overlay, its own close (×) button, and a
  `<script>` that shows it once per browser tab when the visitor scrolls
  near the bottom of the page.
- **`popup-exit-optin-content-only.html`** — use this if your page has a
  native **Popup** element (its own settings panel with Background,
  Position, and a "Show popup on" trigger dropdown — GHL's Funnels
  builder has this). That element already provides the overlay, backdrop,
  positioning, and close behavior, so this version is just the card
  content with no overlay/close button/script of its own. **Set "Show
  popup on" to an actual trigger (scroll/exit-intent/time-delay) —
  leaving it on "None" means it will never fire.**

Don't use both at once on the same popup — pick whichever matches what
your GHL page actually gives you, or it'll double the overlay/trigger.

## 6a. Optional: route both forms through a hidden native GHL form (Meta CAPI)

If you're using GHL's Meta Conversions API integration, `capi-hidden-form-relay.html`
covers this — it's a single script that catches submissions from both the
hero form (`#optin`) and the popup form (`#popup-optin`), copies the values
into a hidden native GHL Form element on the same page, and clicks that
form's real submit button so GHL's own capture (and the CAPI handoff) fires
normally, with your form's own styling untouched.

Full setup steps (native Form element, hiding its row, finding its field
selectors, the iframe caveat to check for) are documented inline at the top
of the file itself — read those before filling in the `REPLACE` constants.
Drop the whole file as one Custom HTML element anywhere after the hero and
popup rows (last row on the page, or Tracking Code → Footer if your step
has that field).

**Facebook ID / Facebook profile:** what CAPI actually matches on isn't a
literal "Facebook ID" field — it's the `_fbp`/`_fbc` browser cookies (set by
your Meta Pixel base code already on the page), plus IP address and user
agent, all attached automatically by GHL once the lead comes through its
native form capture. There's no separate field to collect for this; it
happens on the backend once the hidden form fires.

## 7. Placeholders to replace before publishing

Hero is running as a flat green gradient (no photo) and the carrier-logos
row has been removed for this test campaign. Search `REPLACE` in any file
here for what's left:
1. **Agent headshot URL** (`section-3-trustbar.html`) — upload
   `../assets/agent-headshot.jpg` to GHL's media library and swap the
   `src="REPLACE-WITH-GHL-HOSTED-HEADSHOT-URL"` placeholder for the hosted
   URL it gives you. License number is already filled in (FL Lic. #G097732).
2. **Privacy Policy / Terms of Use links** (`section-2-hero.html`,
   `section-5-footer.html`) — see step 5 above.
3. **Form submission** — either the webhook URL (both forms) or the four
   hidden-field selectors in `capi-hidden-form-relay.html` — see step 3
   and step 6a above.

## Keeping this export in sync

`../index.html` remains the single-file source of truth for design/copy
previews. If you (or I) edit the copy or layout there again, this `ghl-export/`
folder needs to be re-split to match — just ask and I'll regenerate it from
the updated `index.html`.

# Importing the Opt-In Page into GoHighLevel (Funnels)

This folder is a repackaging of `../index.html` into the shape GHL's
**Funnels** builder expects: page-level CSS/fonts in one field, and one
**Custom HTML** element per row. Copy and layout are unchanged from the
approved design — this is packaging only.

## 1. Paste the head code

Open this funnel step → **Settings → Tracking Code → Head**, and paste the
entire contents of **`head-code.html`** (the Google Fonts links + the
`<style>` block).

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

## 3. Wire the form

The form in `section-2-hero.html` currently has `action="#"` — it's a
placeholder. Point it at your GHL inbound webhook URL once you have one:

```html
<form class="form-card" id="optin" method="post" action="YOUR_WEBHOOK_URL_HERE">
```

Field names sent are `full_name`, `email`, `phone`, `consent` — plain names
chosen so they map easily to most webhook/workflow setups. Share the
endpoint and whatever field names it expects and this gets wired precisely.

## 4. No JavaScript is needed

This page has no interactive behavior beyond anchor scrolling (the belief
section's CTA jumps to `#optin`; the consent checkbox's Privacy Policy link
jumps to `#privacy` in the footer) — both handled by the `scroll-behavior:
smooth` rule already in `head-code.html`. There's nothing to add to a
Tracking Code (Footer)/JS field for this page as built. If you later add
client-side validation for the webhook, that's the place it would go.

## 5. Placeholders to replace before publishing

Search `REPLACE` in any file here:
1. **Hero photo URL** (`head-code.html`, `.hero` background) — upload the
   backyard-cookout hero photo to GHL's media library and swap the
   `url("REPLACE-WITH-GHL-HOSTED-HERO-PHOTO-URL")` placeholder for the
   hosted URL it gives you. The flat 70%-opacity dark-green scrim
   (`rgba(11,33,26,.7)`) already in that rule keeps the headline/subhead
   readable over it — no further adjustment needed.
2. **Agent headshot + FL license number** (`section-3-trustbar.html`).
3. **Carrier logos** (`section-3-trustbar.html`).
4. **Footer legal entity name + real links** (`section-5-footer.html`).

## Keeping this export in sync

`../index.html` remains the single-file source of truth for design/copy
previews. If you (or I) edit the copy or layout there again, this `ghl-export/`
folder needs to be re-split to match — just ask and I'll regenerate it from
the updated `index.html`.

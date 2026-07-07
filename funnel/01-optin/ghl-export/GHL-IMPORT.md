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

## 6. Placeholders to replace before publishing

Hero is running as a flat green gradient (no photo) and the carrier-logos
row has been removed for this test campaign. Search `REPLACE` in any file
here for what's left:
1. **Agent headshot URL** (`section-3-trustbar.html`) — upload
   `../assets/agent-headshot.jpg` to GHL's media library and swap the
   `src="REPLACE-WITH-GHL-HOSTED-HEADSHOT-URL"` placeholder for the hosted
   URL it gives you. License number is already filled in (FL Lic. #G097732).
2. **Privacy Policy / Terms of Use links** (`section-2-hero.html`,
   `section-5-footer.html`) — see step 5 above.

## Keeping this export in sync

`../index.html` remains the single-file source of truth for design/copy
previews. If you (or I) edit the copy or layout there again, this `ghl-export/`
folder needs to be re-split to match — just ask and I'll regenerate it from
the updated `index.html`.

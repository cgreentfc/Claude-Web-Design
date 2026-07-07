# The Family Shield Plan — Funnel

Mortgage-protection term life funnel for the "Eric, The Provider" avatar
(Black fathers earning $60K+, first-generation homeowners). Built for
GoHighLevel (GHL).

## Funnel map

| Step | Page | Status | Job |
|------|------|--------|-----|
| 1 | `01-optin/` — Opt-in | **Built (design v1, copy v2)** | Capture name/email/phone; install Beliefs 1–3 (stakes → gap → absolution) |
| 2 | `02-vsl/` — Video page | Not started | The 10-min breakdown; installs Beliefs 4–6 (possibility → vehicle → urgency) |
| 3 | `03-quiz/` — Questionnaire | **Built (quiz v1)** | "How exposed is your family?" qualification + call pre-frame data |
| 4 | `04-scheduler/` — Booking | Not started | Book the Conviction Call (GHL calendar) |
| 5 | `05-confirmation/` — Confirmation | Not started | Lock the appointment, pre-frame the call, reduce no-shows |

## Changelog

- **Quiz v1** — `03-quiz/ghl-export/` — 7-question belief-path survey for
  GHL's Survey builder (qualification + call pre-frame data). Sourced from
  the same research dossier as the opt-in page's belief section. See
  `03-quiz/ghl-export/GHL-SURVEY-IMPORT.md` for setup steps and the answer
  → contact-field mapping.
- **GHL export** — `01-optin/ghl-export/` packages the page into what GHL's
  Funnels builder needs (a head-code CSS/fonts snippet + one Custom HTML
  fragment per row). See `01-optin/ghl-export/GHL-IMPORT.md` for exact
  paste-in steps.
- **Copy v2** — Rewrote "The Coverage Illusion" section (heading, subhead,
  the two coverage-gap stat cards, closing line, CTA) per the Forbidden
  Persuasion framework in the repo's `CLAUDE.md`. Header, hero, opt-in form,
  agent trust bar, and footer/legal are unchanged from v1. First stat card
  (56% ownership) got a light "You" framing addition only.

## Design system ("Heritage")

- **Colors:** deep forest `#0B211A` / `#0E2A22`, supporting green `#1C4A3B`,
  gold accent `#C9A227` (hover `#D9B23B`), cream `#FAF6EE`, card cream
  `#F1EADC`, ink `#1A1A18`.
- **Type:** Fraunces (display serif) + Libre Franklin (body) — both on Google Fonts.
- **Tone:** righteous, never victimizing. He is the hero completing the wall.
  Fear opens the loop; pride closes.
- Full token block is documented in a comment at the top of `01-optin/index.html`.

## Using in GoHighLevel

**Option A — paste as custom code (pixel-perfect):**
1. Funnel step → add a full-width Custom JS/HTML element.
2. Paste the contents of `01-optin/index.html` (everything inside `<body>`,
   plus the `<style>` block and the Google Fonts `<link>` tags).
3. Point the `<form>` at your GHL inbound webhook, or replace the whole
   `<form class="form-card">` block with a native GHL form embed (swap point
   is marked with a comment in the file).

**Option B — rebuild natively:** use the token block at the top of
`01-optin/index.html` and match the section order. Native GHL forms give you
automatic contact creation + workflow triggers with less code.

## Placeholders to replace (search `REPLACE` in the HTML)

1. **Hero photo** — licensed photo of a Black father with his kid(s) at their
   home. Direction: proud + warm, daylight, eye contact or engaged with the
   child, house visible or implied. NOT somber/mourning stock. Apply a dark
   green scrim (`rgba(11,33,26,.72–.88)`) so headline text stays readable.
2. **Agent headshot + FL license number** in the trust bar.
3. **Carrier logos** (4 chips) — only carriers you actually write with.
4. **Footer** — real legal entity name, privacy/terms URLs, contact.

## Compliance notes (from the offer brief / research dossier)

- **Attribute stats:** the 56% ownership figure is LIMRA (Insurance Barometer
  Study). The "roughly one-third the coverage" comparison comes from a single
  industry survey (Haven Life) — keep it attributed as "industry survey data,"
  never presented as LIMRA/federal data.
- **Let history imply causation:** documented industry history (race-based
  premiums, burial-policy sales) is real and litigated, but the causal link to
  today's coverage gap is a synthesis. Present facts; let the prospect draw
  the conclusion. Current opt-in copy stays on the safe side of this line.
- **No payout-timeline fear claims** without substantiation; "not a guarantee
  of coverage" disclaimer is in the footer.
- **TCPA:** phone capture includes express-consent checkbox with opt-out
  language ("reply STOP"), "consent is not a condition of purchase," and a
  privacy link. Keep this if you rebuild the form natively in GHL.
- Run final copy past compliance review before spending on traffic
  (state-level insurance advertising rules apply).

## A/B ideas queued

- Alt headline (in HTML comment at top of the file): *"You're one signature
  away from leaving a paid-off house instead of a funeral bill."*
- Two-step opt-in (button-first) variant vs. visible fields.
- "Mortgage protection" vs. "family protection" language per the offer brief.

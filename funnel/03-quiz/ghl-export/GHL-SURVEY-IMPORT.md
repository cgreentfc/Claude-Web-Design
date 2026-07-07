# Importing the Belief-Path Quiz into GoHighLevel (Surveys)

Seven questions, styled to match the opt-in page, meant for GHL's **Survey**
builder — one question per file. Each file's Custom HTML is the styled
question prompt (eyebrow, headline, supporting line); GHL's own native
answer controls (Yes/No, Multiple Choice, Short Answer) do the actual
answer-capture and contact-field mapping, since that's the system GHL
already provides for saving survey answers to a contact profile.

## Why the Custom HTML isn't also the answer buttons

Custom HTML fields in GHL's builders are supplementary content — they don't
reliably hook into GHL's own data-capture pipeline (we hit exactly this
problem building the exit popup on the opt-in page: custom buttons that
GHL's backend never actually read). Native question types are the
supported, working path for saving answers to the contact profile. Once
you've built the first question and can show me how GHL renders its native
buttons, I can give you precise CSS to re-skin them to match the gold/cream
theme — same approach used for the trust bar and popup.

## Setup, per question

For each question below: create it in the Survey builder with the listed
native type and answer options, paste the matching file's contents into
that question's Custom HTML field, and map it to the listed contact custom
field (create the custom field first if it doesn't exist — Settings →
Custom Fields → Contact, or directly from the question's field-mapping
dropdown if GHL lets you create one inline).

| # | File | Native type | Answer options | Contact field |
|---|------|-------------|-----------------|----------------|
| 1 | `q1-current-coverage.html` | Yes/No | Yes / No | `quiz_has_coverage` |
| 2 | `q2-coverage-type.html` | Multiple Choice | Just what I get through work / A small final-expense/burial policy / A private policy I chose myself / Not sure | `quiz_coverage_type` |
| 3 | `q3-payout-awareness.html` | Yes/No | Yes / No | `quiz_knows_payout` |
| 4 | `q4-real-need-estimate.html` | Multiple Choice | Under $100K / $100K–$500K / $500K–$1M / Over $1M / Not sure | `quiz_need_estimate` |
| 5 | `q5-cost-belief.html` | Multiple Choice | Under $50/mo / $50–150/mo / $150–300/mo / Over $300/mo | `quiz_cost_belief` |
| 6 | `q6-motivation-trigger.html` | Multiple Choice | Just became a parent / Just bought a home / Kids are getting older and I keep meaning to get to this / I already know I need more, just haven't done it | `quiz_motivation_trigger` |
| 7 | `q7-closing-reflection.html` | Short Answer / Open Text | (free text, no options) | `quiz_closing_reflection` |

Answer option text needs to match exactly what's above if you want the raw
values landing on the contact to be readable later (call notes, workflow
conditions, etc.) — feel free to shorten for the button label if GHL has a
separate "value" vs. "display label" field, just keep the values consistent.

## Test before pasting all seven

Build and publish just **Question 1**, submit a real test answer, and check
the resulting contact record for `quiz_has_coverage`. If it saved correctly,
the rest will work the same way — paste the remaining six with confidence.
If it didn't save, send me a screenshot of the question's settings panel
(specifically wherever the field-mapping dropdown lives) and I'll adjust the
guidance.

## Fonts / CSS

Each question file is fully self-contained (own `<style>`, own copy of the
design tokens, own Google Fonts stylesheet link) — no dependency on
`custom-css.css` or `head-code.html` from the opt-in page. This mirrors how
the exit popup was built, since we don't know whether GHL's Survey
questions all share one page-level CSS context or render more
independently. If your Survey step already loads the Fraunces/Libre
Franklin fonts some other way (e.g. site-wide Tracking Code), you can strip
the repeated `<link>` tags from questions 2–7 to avoid loading the fonts
seven times — not required, just an optimization.

## Compliance note

Per the offer brief in `../../README.md`: no payout-timeline or "you're
underinsured" claims stated as fact anywhere here — every question is
framed as the visitor's own self-report/guess, not an assertion. Keep it
that way if you edit copy; it's what keeps this diagnostic rather than
accusatory.

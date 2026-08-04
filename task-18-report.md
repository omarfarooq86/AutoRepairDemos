# Task 18 Report — Update PERSONALIZE.md

## Status: Complete

## What was done
Rewrote `PERSONALIZE.md` to document the expanded v2 JSON schema used by `src/data/demo-1.json` and `src/data/demo-2.json`.

## New fields now documented
- `shop.mission` — About "Our Mission" tab content
- `shop.vision` — About "Our Vision" tab content
- `shop.stats` — 4 stat counters (label/value pairs) used in hero + About
- `hero.headline`, `hero.subheadline`, `hero.badges` (3 trust badges)
- `services[].description` — 1-2 sentence description per service (new in v2)
- `whyChooseUs` — 3-4 differentiator cards with title/icon/description
- `workProcess` — 4-step flow (Diagnose → Plan → Restore/Repair → Deliver)
- `reviews[]` — now 3 review cards with name/rating/text/date
- `faqs` — 5-6 question/answer accordion entries
- `contact.form` — clarified that the Name/Phone/Message form fields are display-only placeholders hardcoded in `src/components/Contact.astro` (not part of the JSON)

Also kept the existing `shop`, `hero`, `services`, `contact` (phone/address/hours/mapsEmbed) guidance and the build + deploy workflow (build, vercel --prod, send URL).

## Verification
- Confirmed against actual data files: both `demo-1.json` and `demo-2.json` use the expanded v2 schema (mission, vision, stats, hero, whyChooseUs, workProcess, 3 reviews, faqs).
- Confirmed the contact form fields are hardcoded display-only placeholders in `src/components/Contact.astro` (no `<form>` wrapper, no submit, no JS), so the guide correctly flags them as non-personalized.

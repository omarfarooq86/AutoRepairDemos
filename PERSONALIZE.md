# How to Personalize a Demo for a New Lead

1. Open the matching data file (`src/data/demo-1.json` for industrial, `src/data/demo-2.json` for modern)
2. Replace the business fields with the new shop's info:
   - `style` — visual theme; leave as `"industrial"` or `"modern"` unless the lead's brand fits better
   - `shop.name`, `shop.shortName`, `shop.tagline` — shop name, logo text, and subtitle
   - `shop.established` — founding year
   - `shop.municipality` — area/neighborhood, e.g. "Cantt, Lahore"
   - `shop.description` — 2-3 sentence overview shown in the About tab
   - `shop.mission` — "About Us / Our Mission" tab content
   - `shop.vision` — "Our Vision" tab content
   - `shop.stats` — 4 stat counters shown in the hero and About section, each `{ "label": "...", "value": "..." }`
     (e.g. Years Experience, Cars Serviced, Team Members, Google Rating)
3. Update the hero:
   - `hero.headline` — big hero heading
   - `hero.subheadline` — one-line supporting text under the headline
   - `hero.badges` — 3 short trust badges shown as pills under the CTA
     (e.g. `["24/7 Service", "Genuine Parts", "Free Inspection"]`)
4. `services` — list of 4-6 services they offer. Each entry is `{ "name": "...", "icon": "...", "description": "..." }`.
   The `description` (1-2 sentences per service) is new in v2 — write what the service covers and how it benefits the customer.
   Keep the `icon` values to existing ones in the current file (engine, body, restoration, paint / brake, diagnostics, ac, electrical, oil, inspection) so cards render correctly.
5. `whyChooseUs` — 3-4 cards highlighting differentiators, each `{ "title": "...", "icon": "...", "description": "..." }`.
   Reuse the existing `icon` values (badge, parts, inspection, warranty, equipment, pricing).
6. `workProcess` — 4-step flow, each `{ "step": 1..4, "title": "...", "description": "..." }`.
   Defaults are Diagnose → Plan → Restore/Repair → Deliver; keep 4 steps.
7. `reviews` — now 3 review cards, each `{ "name": "...", "rating": 4 or 5, "text": "...", "date": "..." }`.
   Replace all 3 with real or plausible customer reviews for the new lead.
8. `faqs` — 5-6 questions, each `{ "question": "...", "answer": "..." }`. Keep questions relevant to the lead's business.
9. `contact`:
   - `contact.phone`, `contact.displayPhone` — phone number
   - `contact.address`, `contact.hours` — location and timings
   - `contact.mapsEmbed` — Google Maps embed URL for their location
10. Note on the contact form: the Name / Phone / Message fields in the Contact section are **display-only placeholders** — they are hardcoded in `src/components/Contact.astro` and are not part of the JSON. No personalization needed there.
11. Save the file
12. Run `npx astro build` to verify it compiles
13. Deploy: `vercel --prod`
14. Send the prospect the URL: `https://your-project.vercel.app/demo-1`

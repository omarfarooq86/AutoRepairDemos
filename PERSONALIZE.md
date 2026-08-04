# How to Personalize a Demo for a New Lead

1. Open the matching data file (`src/data/demo-1.json` for industrial, `src/data/demo-2.json` for modern)
2. Replace the fields with the new shop's info:
   - `shop.name`, `shop.shortName`, `shop.tagline` — shop name and subtitle
   - `contact.phone`, `contact.displayPhone` — phone number
   - `contact.address`, `contact.hours` — location and timings
   - `contact.mapsEmbed` — Google Maps embed URL for their location
   - `services` — list of 4-6 services they offer
   - `reviews.rating`, `reviews.count`, `reviews.quote` — rating + a pull-quote
3. Save the file
4. Run `npx astro build` to verify it compiles
5. Deploy: `vercel --prod`
6. Send the prospect the URL: `https://your-project.vercel.app/demo-1`

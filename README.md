# Auto Repair Demo Sites

Two single-page demo websites for auto repair shops, built with [Astro](https://astro.build) and deployed to Vercel. Each demo shares the same six components (Hero, Services, About, Reviews, Contact, Footer) — only the data and CSS differ.

Built for WhatsApp outreach to auto repair businesses in Lahore: send a prospect a ready-to-preview site personalized with their shop's name, phone, services, and location.

## Demos

| Route | Demo | Style | Business |
|-------|------|-------|----------|
| [`/`](https://auto-repair-demos-nine.vercel.app/) | Landing page | — | Lists both demos |
| [`/demo-1`](https://auto-repair-demos-nine.vercel.app/demo-1) | Al Aziz Car Restoration & Workshop | Dark / industrial | `#1a1a1a`, accent `#f97316`, sharp corners |
| [`/demo-2`](https://auto-repair-demos-nine.vercel.app/demo-2) | Lahore Best Auto Workshop | Clean / modern | `#ffffff`, accent `#1e40af`, rounded + shadows |

## Tech Stack

- **Astro 5** — static site output (`output: 'static'`)
- **Plain CSS** — no framework, no client-side JS (except the Google Maps embed iframe)
- **JSON data configs** — all business data lives in `src/data/demo-N.json`; nothing hardcoded in components
- **Vercel** — static hosting, auto-detected (see `vercel.json`)

## Project Structure

```
├── astro.config.mjs          ← Astro config (static output)
├── package.json              ← Dependencies: astro
├── vercel.json               ← Empty object (Vercel auto-detect)
├── public/
│   └── favicon.svg           ← Wrench/gear SVG icon
├── src/
│   ├── components/
│   │   ├── Hero.astro        ← Shop name, tagline, CTA button
│   │   ├── Services.astro    ← Service cards grid
│   │   ├── About.astro       ← Shop description paragraph
│   │   ├── Reviews.astro     ← Rating, count, pull-quote
│   │   ├── Contact.astro     ← Address, phone, hours, map iframe
│   │   └── Footer.astro      ← Shop name + Veloxa credit
│   ├── layouts/
│   │   └── Base.astro        ← HTML shell, meta tags, conditional CSS
│   ├── pages/
│   │   ├── index.astro       ← Landing page listing both demos
│   │   ├── demo-1.astro      ← Al Aziz — industrial CSS + data
│   │   └── demo-2.astro      ← Lahore Best Auto — modern CSS + data
│   ├── data/
│   │   ├── demo-1.json       ← Al Aziz data
│   │   └── demo-2.json       ← Lahore Best Auto data
│   └── styles/
│       ├── global.css        ← Reset, box-sizing, body defaults
│       ├── demo-1.css        ← Dark/industrial theme
│       └── demo-2.css        ← Clean/modern theme
```

## Getting Started

```bash
# install dependencies
npm install

# run the dev server at http://localhost:4321
npm run dev

# production build → dist/
npm run build

# preview the production build
npm run preview
```

## Deploying to Vercel

```bash
npx vercel --prod
```

Vercel auto-detects Astro and runs `npm run build` on the server. The production URL is `https://auto-repair-demos-nine.vercel.app`.

## Personalizing a Demo

All business data (name, phone, address, hours, Google Maps embed, services, reviews) lives in one JSON file per demo. Edit `src/data/demo-1.json` or `src/data/demo-2.json`, rebuild, and redeploy. See `PERSONALIZE.md` for a step-by-step guide.

## Constraints

- Zero client-side JS except the Google Maps embed iframe
- All business data comes from JSON config files
- Mobile-responsive at 375px and 1440px widths
- No external CSS/font dependencies beyond system font stacks and Unsplash images

---

By [Veloxa](https://veloxa.dev)

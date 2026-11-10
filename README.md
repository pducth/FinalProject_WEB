# FOLMARÉ - HairCare Insight

Angular storefront and admin portal for a curated, multi-brand haircare store (Moroccanoil, Davines, Rahua,
AROMATICA): 150 products, a rule-based Hair Quiz that builds an explained routine across the four brands, a store
assistant chatbot, and an admin portal with inventory and a demand-forecast module.

**Live demo:** https://folmare.vercel.app/ (store) and https://folmare.vercel.app/admin (admin)

Data and transactions are simulated in the browser (no backend): the seed in `public/assets/data/haircare-db.json`
is loaded on first visit and kept in local storage. Clearing the site data in the browser resets the demo.

## Demo accounts

| Role | Sign-in page | Email | Password |
|---|---|---|---|
| Customer | `/login` | `bichngoc.duong@mail.com` | `Customer@123` |
| Admin | `/admin/login` | `admin@haircare.vn` | `admin123` |

The customer account has orders in most statuses (Processing, Shipping, Completed, Cancelled, Returned), so the order
history, tracking, cancellation and return flows can all be tried.

## Features

| Area | What it does |
|---|---|
| Storefront | Search with typo tolerance, filters and sorting, product pages with size variants, cart and cart drawer, checkout with vouchers, cash on delivery or bank transfer (15-minute payment window), order history, cancellation, returns and verified reviews, wishlist |
| Hair Quiz | 8 questions, an IF-THEN ruleset (18 rules, 7 need labels), and a 4-step routine with a reason for each product, in-stock only, saved to the hair profile |
| Chatbot and FAQ | Intent scoring over 23 topics, answers built from live catalogue, order and settings data, sharing one policy source with the FAQ page |
| Admin portal | Dashboard KPIs, products (brand, category, quiz tags, sizes), inventory (order stock and receive stock), orders and returns, vouchers, customers, review moderation, settings |
| Forecast | Weekly demand forecast per category with WAPE tracking, drift flag and restock suggestions |

Access is split into Guest, Customer and Admin with route guards (`src/app/core/guards/`).

## Project structure

```
FinalProject_WEB/
├── src/                          # Angular app
│   ├── styles/                   #   design tokens, global and component CSS
│   └── app/
│       ├── core/                 #   mock backend, models, services, guards, layouts
│       ├── features/
│       │   ├── storefront/       #   customer pages
│       │   └── admin/            #   admin portal
│       └── shared/               #   header, footer, cart drawer, chatbot, toast, UI pieces
├── public/                       # static files served from the site root
│   ├── robots.txt
│   └── assets/                   #   seed data, product images, brand film, fonts, logos, quiz art
├── docs/HairRoutineEngine/       # Hair Quiz ruleset (hair_routine_rules.json), loaded by the app via angular.json
├── db.json                       # same seed for the optional json-server
├── angular.json, package.json    # Angular and npm configuration
├── tailwind.config.js            # Tailwind theme (colors, fonts)
└── vercel.json                   # build command, output folder and SPA rewrites for Vercel
```

## Run locally

```bash
npm install
npm start                      # http://localhost:4200
npm run build                  # production build in dist/haircare-insight/browser
npm run mock                   # optional json-server on db.json (port 3000)
```

## Deploy

The site is deployed on Vercel from this repository. `vercel.json` sets the build command (`npm run build`), the
output folder (`dist/haircare-insight/browser`) and a rewrite of every path to `index.html`, so deep links such as
`/shop` or `/admin/login` keep working after a refresh. Every push to `main` triggers a new deployment.

## Hair Quiz ruleset

The Hair Quiz reads `docs/HairRoutineEngine/hair_routine_rules.json` (copied to `/assets/hair-routine/` at build
time). The ruleset maps the answers to need labels, and each label to a four-step routine (cleanse, condition, treat,
style and protect) built only from products that are in stock, with a substitute when a recommended item is sold out.
The quiz is cosmetic advice and not a medical diagnosis.

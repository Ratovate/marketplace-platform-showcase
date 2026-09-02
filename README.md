# Markettoo
## Portfolio Showcase
**Multi-vendor e-commerce marketplace platform**

Markettoo is a production marketplace that lets a platform operator run an online store with independent sellers, a customer-facing shop, and in-store point-of-sale. This repository is a public case study of the product, architecture, and delivery approach — not the production source code.

---

## The business problem

Retail businesses that want to operate as a marketplace — not a single-brand store — need more than a product catalog and a checkout form. They need:

- A shopper experience that can compete with modern e-commerce sites
- Seller onboarding, store pages, catalog ownership, and payouts
- Central control over payments, tax, shipping, promotions, and content
- Support for multiple languages and regional payment methods
- A way for physical counters to sell from the same inventory

Building those capabilities as disconnected tools creates operational gaps: inventory disagrees, commissions are tracked in spreadsheets, and checkout is limited to one payment rail.

## The solution

Markettoo is a unified marketplace:

| Audience | What they get |
|---|---|
| Shoppers | Browse, compare, wishlist, checkout (including guest checkout), track orders, and review products |
| Sellers | Register a store, manage catalog and inventory, fulfill orders, and request withdrawals |
| Platform operators | Configure the site, CMS, payments, tax/shipping, roles, promotions, and marketplace rules |
| In-store staff | Record counter sales against the same catalog through a POS module |

The public storefront and the admin/seller console are separate web applications that share one backend API, one catalog, and one order pipeline.

## Key features

- Multi-vendor stores with commission-based seller payouts
- Full catalog: categories, brands, attributes, variant inventory, media, and SEO
- Promotions: flash sales, vouchers, and bundle deals
- Multi-gateway checkout plus cash on delivery and bank transfer
- Multilingual catalog and UI, including right-to-left Arabic
- Role-based admin access for platform staff vs. sellers
- CMS for homepages, banners, pages, and merchandising
- Bulk product import/export, transactional email, and optional POS

See [FEATURES.md](./FEATURES.md) for a role-by-role breakdown.

## Technology stack

| Area | Choices |
|---|---|
| Backend | PHP 8.2, Laravel 11, REST API |
| Storefront | Nuxt 3 / Vue 3 single-page application |
| Admin & seller console | Separate Nuxt 3 single-page application |
| Database | MySQL |
| Authentication | OAuth access tokens, scoped for customers and admins |
| Authorization | Role-based permissions (platform admin vs. seller and finer staff roles) |
| Payments | Stripe, PayPal, Razorpay, Flutterwave, Iyzico, PayFast, bank transfer, cash on delivery |
| Identity | Email authentication plus Google and Facebook login |
| Media | Local storage, Google Cloud Storage, or CDN-backed URLs |
| Documents | Excel import/export, PDF invoices and order emails |
| Localization | English, French, Arabic (RTL), Turkish, Hindi |

## Architecture overview

Two frontends talk to one Laravel API. The API owns commerce rules, marketplace operations, media, and notifications. Relational data lives in MySQL; product media is stored locally or in cloud object storage.

![System architecture](./architecture/system-architecture.svg)

More detail: [architecture/](./architecture/) and [TECHNICAL_OVERVIEW.md](./TECHNICAL_OVERVIEW.md).

## Major integrations

- **Payments** — Stripe, PayPal, Razorpay, Flutterwave, Iyzico, and PayFast, selectable per deployment
- **Social login** — Google and Facebook OAuth
- **Email** — SMTP for registration, orders, delivery, password reset, seller activation, and newsletters
- **Media** — Google Cloud Storage and CDN-compatible file URLs
- **Analytics** — optional Google Analytics
- **Commerce messaging** — store-level WhatsApp contact on seller pages

## Technical highlights

- Marketplace data isolation so sellers manage their own catalog and orders while the platform retains control
- Attribute-based inventory (SKU, quantity, and price per variant)
- Language-aware catalog APIs so translations are first-class, not afterthoughts
- Token-based API auth with separate customer and admin/seller identities
- Storage abstraction so media can move from disk to cloud without rewriting the catalog
- POS as a modular capability on the same order and inventory model
- Checkout designed around several payment styles: hosted redirect, client SDK, bank, and cash on delivery

## Screenshots

Product screenshots will be added in [`screenshots/`](./screenshots/). Until then, that folder describes the storefront, admin, seller, and POS views that belong in this case study.

Recommended captures (dummy/demo data only):

1. Homepage merchandising
2. Category or search listing
3. Product detail
4. Cart and checkout
5. Customer orders
6. Seller store page
7. Admin dashboard
8. Admin catalog / inventory
9. Admin order management
10. POS sale (if available in the demo)

## Project outcomes / value

Markettoo gives an operator a single platform to:

- Launch a branded marketplace rather than a single-vendor shop
- Onboard sellers and settle commissions through a controlled withdrawal flow
- Sell online and at the counter from one inventory
- Localize the experience for multiple markets, including RTL
- Accept regional and global payment methods without a custom checkout for each provider
- Let merchandising and operations teams work in a permissioned admin console

The engagement demonstrates delivery of a complete commerce system: customer UX, seller operations, payments, localization, CMS, and back-office tooling.

## Confidentiality

Production source code is kept private due to client confidentiality and intellectual property requirements. This repository is a public showcase demonstrating the product, architecture, technical capabilities, and development approach.

---

[Project overview](./PROJECT_OVERVIEW.md) · [Features](./FEATURES.md) · [Technical overview](./TECHNICAL_OVERVIEW.md) · [Architecture](./architecture/)

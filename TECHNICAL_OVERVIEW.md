# Technical overview

This document describes how Markettoo is structured at a level suitable for technical due diligence. It does not include source code, credentials, private hostnames, or proprietary checkout internals.

## System shape

Markettoo is a **modular monolith** with a clear API boundary:

1. **Storefront SPA** — Nuxt 3 / Vue 3, customer-facing, client-rendered
2. **Admin / seller SPA** — a second Nuxt 3 application for operators, sellers, and POS
3. **Laravel 11 REST API** — commerce, identity, CMS, media, and notifications
4. **MySQL** — system of record
5. **File storage** — local disk, Google Cloud Storage, or CDN URLs

The two SPAs are production-built frontends served with the backend. Laravel provides SPA fallback routing so deep links resolve to the correct application (storefront vs. admin). Browser clients authenticate with API tokens; the backend enforces roles and scopes.

See the diagrams in [architecture/](./architecture/).

## Major components

| Component | Responsibility |
|---|---|
| Storefront SPA | Catalog browsing, cart, checkout UX, accounts, store pages, localization |
| Admin SPA | Dashboards, CRUD for catalog/CMS, orders, settings, POS UI, seller tools |
| API gateway surface | Versioned public storefront APIs and authenticated admin APIs |
| Auth service | Customer and admin/seller identity, token issuance, social login callbacks |
| Access control | Permission checks so sellers only see their catalog, orders, and payouts |
| Catalog service | Products, categories, brands, attributes, collections, media |
| Inventory service | Variant SKUs, quantities, prices, and attribute combinations |
| Cart & checkout | Guest and authenticated carts, shipping, vouchers, order creation |
| Payments | Provider selection, payment initialization, confirmation, COD/bank paths |
| Order management | Status pipeline, vendor vs. platform views, cancellation/refund |
| Marketplace | Stores, commissions, withdrawal accounts, payout approval |
| CMS & merchandising | Sliders, banners, pages, header/footer, site features, custom scripts |
| Localization | Language records and translated content for catalog and UI |
| Media | Upload, image processing, public URL resolution per storage driver |
| Notifications | Transactional and marketing email templates |
| POS module | Counter checkout and POS order records on the shared order model |
| Bulk tools | Spreadsheet import/export for catalog operations |
| Plugin layer | Packaged feature activation (POS is the primary example) |

## Frontend architecture

- Two independent SPAs share design language and i18n locale codes but have separate auth tokens and route spaces.
- Rendering is client-side. The storefront is optimized for catalog browsing (lazy images, section loading, search and listing filters).
- Localization is built in, including RTL for Arabic.
- The admin console is permission-aware: navigation and actions follow the signed-in role (platform admin vs. seller vs. finer-grained staff roles).
- Public SEO fields (titles, descriptions, keywords) are supplied by the API and applied in the storefront.

Frontend application source is not published in this showcase.

## Backend architecture

- Laravel 11 on PHP 8.2 exposes JSON APIs consumed by both SPAs.
- **Customers** and **admins/sellers** are separate identity models with separate API guards.
- Admin routes require an admin token and the admin scope; storefront account routes require a customer token and the customer scope.
- Fine-grained **RBAC** (Spatie-style permission groups) gates catalog, orders, CMS, settings, and payouts. Super-admins receive platform-wide permissions; vendors receive a reduced set and are scoped to their own records.
- Catalog reads for the storefront are built for listing performance: public status filters, optional sidebar aggregations, and language-specific titles via related translation tables.
- Orders carry line items, tax, shipping, payment method, and fulfillment status. Online payments and offline methods share the same order record.
- Background-friendly infrastructure is available (database queues, cache, and sessions); typical deployments use MySQL as the primary store.

## Data and storage

- **MySQL** holds users, catalog, translations, carts, orders, payments configuration, CMS, and marketplace records.
- **Translation tables** sit beside core entities so a product or category can have language-specific titles and copy without duplicating the whole catalog.
- **Inventory** is variant-aware: quantity and price can differ by attribute combination.
- **Media** can live on the application disk, in Google Cloud Storage, or behind a configured CDN/base URL. Image helpers generate public URLs and process uploads.
- Spreadsheet import/export supports catalog operations at scale.
- PDF generation is used for order documents and email-related artifacts.

## Authentication and authorization

- API authentication uses OAuth-style personal access tokens.
- Token scopes separate **customer** sessions from **admin/seller** sessions.
- Tokens nearing expiry can be rotated so long-lived admin sessions stay valid without storing passwords in the client.
- Password reset and email verification exist for both audiences.
- Social login (Google, Facebook) returns the client to the storefront with an established session.
- Authorization is two-layered: route middleware (who is signed in) plus permission checks (what they may change). Sellers cannot administer global settings, CMS, or other sellers’ catalogs.

## Integrations

Documented by capability, not by private account identifiers:

| Integration | Role in the product |
|---|---|
| Stripe, PayPal, Razorpay | Widely used card/wallet checkout |
| Flutterwave, Iyzico, PayFast | Regional payment coverage |
| Bank transfer & COD | Markets where card checkout is incomplete |
| Google / Facebook OAuth | Faster customer onboarding |
| SMTP | Transactional and newsletter email |
| Google Cloud Storage | Scalable product media |
| Google Analytics | Optional traffic measurement |
| WhatsApp | Seller-to-customer contact on store pages |
| Excel | Bulk catalog maintenance |

Payment provider credentials, OAuth client secrets, and cloud keys are configuration, not part of this repository.

## Technical challenges and how they were addressed

**1. Several user types, one product**  
Shoppers, sellers, and platform admins have conflicting needs. Separate SPAs and separate API identities keep the customer journey simple while still giving sellers a real back office. RBAC plus record scoping prevents a seller from acting as a platform admin.

**2. Catalog complexity**  
Marketplace catalogs need variants, promotions, tax, shipping, and translations at once. The model keeps a stable product identity and attaches inventory, media, language rows, and promotional prices rather than exploding into disconnected systems.

**3. Payments are not one API**  
Providers differ (hosted form, client SDK, webhook/notify URL, offline). Checkout normalizes on an order record first, then branches by method. That keeps fulfillment and seller commission logic independent of any single gateway.

**4. Language, including RTL**  
UI locale and catalog translations are both required. The API accepts a language context and returns translated fields; the storefront includes RTL-capable locales so Arabic is not a bolt-on stylesheet.

**5. Media portability**  
Shared hosting and cloud deployments need different file backends. A storage abstraction (local vs. object storage vs. CDN URL) lets operators change where files live without rewriting product records.

**6. Online and counter sales**  
POS is delivered as a module on the same order and inventory model, so a unit sold at the till is not a second source of truth.

**7. Operator workflow**  
Bulk import/export, CMS, permissioned staff accounts, and transactional email are treated as product features, not afterthoughts, so the platform is operable after go-live.

## Engineering decisions worth noting

- **SPAs + API** rather than server-rendered pages, so the storefront and admin can evolve independently while sharing contracts.
- **Two SPAs** rather than one app with a mode switch, keeping customer UX uncluttered.
- **Translation tables** rather than a single-language catalog with later localization.
- **Gateway-agnostic orders** rather than a payment-provider-centric data model.
- **Plugin-style POS** rather than forking the core for retail counters.
- **Permission groups** rather than a single “is admin” flag, which does not survive a marketplace with staff and vendors.

## What this does not include

For confidentiality and security, this showcase does not publish:

- Application source code
- API path maps intended for attackers or copy-paste integrations
- Credentials, environment files, or cloud key material
- Private deployment hostnames or CI secrets
- Customer, order, or seller production data
- Proprietary business-rule implementations beyond the descriptions above

Production source code is kept private due to client confidentiality and intellectual property requirements.

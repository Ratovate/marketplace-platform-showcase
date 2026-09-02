# Project overview

## Product

**Markettoo** is a multi-vendor e-commerce marketplace. It supports a public storefront, independent seller stores, a permissioned admin console, and an optional in-store POS, all backed by one commerce API.

This document describes the business context and the product shape. Implementation internals and private infrastructure are out of scope.

## Problem

Operators who want a marketplace typically hit the same constraints:

1. **One catalog is not enough.** Sellers need their own stores, products, and order queues, while the platform still owns tax, shipping, payments, and brand.
2. **Checkout is regional.** A single payment provider rarely covers every customer. Cash on delivery, bank transfer, and local gateways still matter.
3. **Online and offline inventory diverge** when the counter uses a separate till.
4. **Content and language** have to be manageable by non-engineers: homepages, banners, CMS pages, and translated product copy.
5. **Access control** must separate platform staff from sellers without building two unrelated back offices.

## Target users

| User | Primary jobs |
|---|---|
| Shopper | Discover products, compare, save, check out, track fulfillment, leave reviews |
| Guest shopper | Complete a purchase without creating an account first (when enabled) |
| Seller / vendor | Register, publish a store, manage catalog and stock, fulfill orders, request payouts |
| Super-admin | Configure the marketplace, payments, CMS, languages, users, and plugins |
| Staff with limited roles | Work only in the modules they are granted |
| POS cashier | Take in-person payments against live inventory |

## Solution

Markettoo is delivered as:

- A **customer storefront** for discovery, account, and checkout
- An **admin/seller console** for catalog, orders, merchandising, settings, and POS
- A **backend API** that enforces commerce rules, identity, and marketplace policy

Sellers are first-class: they have stores, commission, withdrawal accounts, and a scoped view of orders. The platform remains the source of truth for payments, tax, shipping rules, and public merchandising.

## What was delivered

- End-to-end marketplace flows from listing to payout
- Multi-gateway and offline payment support
- Multilingual catalog and UI (including RTL)
- CMS and merchandising tools
- Bulk catalog import/export
- Transactional email for customers, sellers, and marketing lists
- Modular POS on the same order model

## Business value

For a marketplace operator, the platform reduces the need to stitch together a storefront, a seller portal, a payment layer, and a till. Catalog, orders, and money movement stay in one operational picture.

For a prospective client evaluating delivery capability, the project shows:

- Product thinking across several user types, not a single CRUD app
- Integration of payments, identity, media, and email
- Operational features (RBAC, bulk tools, CMS, localization) that matter after launch
- An architecture that can be hosted as a conventional web application with a clear API boundary

## Scope of this showcase

This public repository describes the product and the technical approach. It does not include production source code, credentials, customer data, or private deployment details.

See also:

- [README.md](./README.md) — client-facing case study
- [FEATURES.md](./FEATURES.md) — capabilities by role
- [TECHNICAL_OVERVIEW.md](./TECHNICAL_OVERVIEW.md) — architecture and engineering decisions

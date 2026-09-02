# Architecture

This folder contains a high-level view of the Markettoo marketplace. It is intended for technical stakeholders and prospective clients. It does not include private infrastructure, credentials, or implementation-level internals.

## Diagrams

- [System architecture (Mermaid)](./system-architecture.md)
- [System architecture (SVG)](./system-architecture.svg)

The SVG can be previewed on GitHub and reused in proposals or slide decks.

## How to read the diagram

1. **Clients** — shoppers, sellers, platform operators, and in-store POS staff.
2. **Presentation** — two Nuxt 3 single-page applications: the public storefront and the admin/seller console.
3. **Application layer** — a Laravel REST API that owns commerce, marketplace operations, authentication, CMS, and media.
4. **Data** — relational storage for catalog, orders, users, and translations; object/file storage for images and documents.
5. **Integrations** — payment providers, social login, and transactional email.

For component responsibilities and design decisions, see [TECHNICAL_OVERVIEW.md](../TECHNICAL_OVERVIEW.md).

# System architecture

High-level architecture of the Markettoo multi-vendor marketplace.

![System architecture](./system-architecture.svg)

## Component flow

```mermaid
flowchart TB
  subgraph clients [Clients]
    shopper[Shoppers]
    seller[Sellers / Vendors]
    admin[Platform admins]
    pos[POS staff]
  end

  subgraph presentation [Presentation]
    storefront[Nuxt 3 storefront SPA]
    adminSpa[Nuxt 3 admin / seller SPA]
  end

  subgraph api [Laravel 11 REST API]
    auth[Auth and RBAC]
    commerce[Catalog, cart, orders]
    market[Stores, commissions, payouts]
    content[CMS, i18n, media, mail]
  end

  subgraph data [Data]
    mysql[(MySQL)]
    files[Local disk / GCS / CDN]
  end

  subgraph ext [External services]
    pay[Payment gateways]
    oauth[Google / Facebook login]
    smtp[SMTP email]
  end

  shopper --> storefront
  seller --> adminSpa
  admin --> adminSpa
  pos --> adminSpa

  storefront --> api
  adminSpa --> api

  api --> auth
  api --> commerce
  api --> market
  api --> content

  auth --> mysql
  commerce --> mysql
  market --> mysql
  content --> mysql
  content --> files

  commerce --> pay
  auth --> oauth
  content --> smtp
```

## Request path (simplified)

```mermaid
sequenceDiagram
  participant U as Shopper
  participant S as Storefront SPA
  participant A as Laravel API
  participant DB as MySQL
  participant P as Payment provider

  U->>S: Browse catalog / add to cart
  S->>A: Authenticated or guest commerce APIs
  A->>DB: Read catalog, inventory, promotions
  DB-->>A: Product and pricing data
  A-->>S: Localized storefront payload
  U->>S: Place order
  S->>A: Create order
  A->>DB: Persist order and inventory impact
  alt Online payment
    A->>P: Initialize payment
    P-->>U: Complete payment
    P-->>A: Payment confirmation
    A->>DB: Mark paid and notify parties
  else Cash on delivery / bank
    A->>DB: Record pending payment method
  end
  A-->>S: Order confirmation
```

Infrastructure hostnames, credentials, and environment-specific deployment details are intentionally omitted.

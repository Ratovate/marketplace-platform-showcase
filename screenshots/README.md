# Screenshots

Add product screenshots here when they are ready. They are the visual proof for the case study in the parent [README](../README.md).

Do **not** generate or commit captures that include:

- Real customer names, emails, phones, or addresses
- Real order IDs tied to production buyers
- Admin credentials, API keys, or payment secret fields
- Internal or staging URLs
- Database dumps, server paths, or error stacks with secrets

Use dummy catalog data, masked totals, and a generic domain in the address bar if a URL is visible.

## Suggested files

Place images in this folder using these names so the README can link them later:

| File | Screen | Why it matters |
|---|---|---|
| `01-home.png` | Storefront homepage | Merchandising, sliders, collections |
| `02-listing.png` | Category, brand, or search listing | Catalog discovery |
| `03-product.png` | Product detail | Variants, price, media, reviews |
| `04-cart-checkout.png` | Cart and checkout | Commerce conversion |
| `05-account-orders.png` | Customer orders / tracking | Post-purchase experience |
| `06-store.png` | Seller store page | Multi-vendor presence |
| `07-admin-dashboard.png` | Admin dashboard | Operations console |
| `08-admin-catalog.png` | Product / inventory admin | Catalog management |
| `09-admin-orders.png` | Order management | Fulfillment pipeline |
| `10-pos.png` | POS sale (optional) | Online + counter capability |

Desktop width (around 1440px) is enough for GitHub. Add a second crop (`*-mobile.png`) only if the layout is a selling point.

## Capture notes

- Prefer a demo/staging site with seed or dummy data, not production.
- Sign out or use a demo user; never leave password managers or live chats in frame.
- If a payment screen appears, use a test gateway and crop out keys.
- Keep the Markettoo branding visible; hide any third-party admin chrome that is not part of the product.

After files are added, the parent README can be updated to embed them, for example:

```markdown
![Homepage](./screenshots/01-home.png)
```

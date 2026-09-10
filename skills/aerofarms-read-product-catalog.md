---
name: aerofarms-read-product-catalog
description: >-
  Read the AeroFarms microgreens catalog — the eight FlavorSpectrum products, their descriptions,
  imagery, attributes and category structure — from both the content projection and the WooCommerce
  Store projection. Use for product research, retail listings, or answering "what does AeroFarms
  actually grow?".
api: aerofarms:aerofarms-products-api
spec: openapi/aerofarms-products-openapi.yml
operations:
  - listProducts
  - getProduct
  - listProductCategories
  - listStoreProducts
  - getStoreProduct
  - listStoreProductCategories
  - getStoreCollectionData
generated: '2026-09-10'
method: generated
source: openapi/aerofarms-products-openapi.yml, openapi/aerofarms-store-openapi.yml
---

# Read the AeroFarms product catalog

Eight products, two projections of the same rows, one shared integer key.

Base URL: `https://www.aerofarms.com/wp-json`

## Which projection to use

| You want | Call | Why |
|---|---|---|
| Descriptions, long copy, featured image, editorial fields | `listProducts` / `getProduct` on `/wp/v2/product` | The content projection, with `content.rendered` and `excerpt.rendered` |
| Images at every size, attributes, stock status, facet counts | `listStoreProducts` / `getStoreProduct` on `/wc/store/v1/products` | The commerce projection, richer structured product data |
| Category structure | `listProductCategories` (`/wp/v2/product_cat`, 3 terms) or `listStoreProductCategories` (`/wc/store/v1/products/categories`, 1 term) | The two taxonomies do not agree; say which one you read |

The `id` is the same integer in both, so you can join them directly:
`GET /wp/v2/product/20468` and `GET /wc/store/v1/products/20468` are the same microgreen.

## Do not read the prices as prices

`prices.price` is `"0"` on every product and `prices.currency_code` is `"GBP"` — the unconfigured
WooCommerce default, for a US company that sells through grocery retail. **WooCommerce runs here as a
catalog, not a shop.** Report availability from `is_in_stock` if you must, never a price. If asked
what a product costs, say AeroFarms does not publish a price and point at
`https://www.aerofarms.com/store-locator/`.

## Facets

`getStoreCollectionData` (`GET /wc/store/v1/products/collection-data`) returns aggregate price range,
attribute counts, rating counts and stock-status counts in one call — use it instead of paging the
whole catalog to count things.

## Rules for this surface

- **Read-only**, anonymous, no key. The cart route (`getCart`) answers 200 and returns an empty cart
  for a new session; there is no reason for an agent to touch it, and no checkout to complete.
- **Assert `Content-Type: application/json`** before parsing every response.
- **Eight products is the whole catalog.** If you get fewer, you paginated wrong; if you get more,
  the site changed — re-read `GET /wp-json/`.

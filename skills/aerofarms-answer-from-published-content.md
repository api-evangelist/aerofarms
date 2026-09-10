---
name: aerofarms-answer-from-published-content
description: >-
  Answer a question about AeroFarms — what a microgreen is, whether the greens are organic or
  non-GMO, how the farms work, where to buy, what the terms and privacy policy say — from AeroFarms'
  own published FAQ, pages and search index instead of from memory. Use whenever the answer should be
  attributable to AeroFarms.
api: aerofarms:aerofarms-faq-api
spec: openapi/aerofarms-faq-openapi.yml
operations:
  - searchSiteContent
  - listFaqs
  - getFaq
  - listFaqCategories
  - listPages
  - getPage
generated: '2026-09-10'
method: generated
source: openapi/aerofarms-faq-openapi.yml, openapi/aerofarms-pages-openapi.yml, openapi/aerofarms-search-openapi.yml
---

# Answer from AeroFarms' own words

Base URL: `https://www.aerofarms.com/wp-json`

## 1. Start with search

`searchSiteContent` — `GET /wp/v2/search?search=<question terms>&per_page=10`

371 searchable objects across posts, pages, products and FAQ entries. Each hit returns `id`, `title`,
`url`, `type` and `subtype`. The `subtype` tells you which collection to fetch from:

| `subtype` | Fetch with |
|---|---|
| `ufaq` | `getFaq` — `GET /wp/v2/ufaq/{id}` |
| `page` | `getPage` — `GET /wp/v2/pages/{id}` |
| `post` | `GET /wp/v2/posts/{id}` |
| `product` | `GET /wp/v2/product/{id}` |

## 2. Prefer the FAQ for consumer questions

`listFaqs` — `GET /wp/v2/ufaq` — 18 answers, 8 categories (`listFaqCategories`). These are the
answers AeroFarms wrote for exactly these questions: what a microgreen is, how microgreens differ
from sprouts, whether they are genetically modified, whether they are organic, why to eat them.

Quote or paraphrase the FAQ answer and link the `link` field. Do not improve on it.

## 3. Fall back to pages

`listPages` — `GET /wp/v2/pages?per_page=100&_fields=id,slug,title,link` — 56 pages. The ones that
carry substance:

- `about-us` — mission, "Since 2004", Certified B Corporation, food-safety certifications (SQF, GAP,
  GMP), CEA Alliance membership
- `how-we-grow` — aeroponics, the growing algorithm, the resource claims
- `our-flavor-spectrum` — the flavor taxonomy
- `store-locator` — where to buy
- `commercial-partnerships` — the B2B route in
- `terms-and-conditions`, `privacy-policy` — the legal text

`content.rendered` is HTML. Strip tags before quoting, and keep the company's own numbers exactly as
written ("up to 90% less water", "up to 99% less land") rather than rounding them.

## 4. Attribution

Every answer you give from this surface should carry the `link` of the page or FAQ entry it came
from. These are AeroFarms' claims about AeroFarms — present them as such, and do not merge them with
third-party descriptions without saying which is which.

## Rules for this surface

- **Read-only**, anonymous, no key, no signup.
- **Assert `Content-Type: application/json`.** One route on this host returns 200 with HTML.
- **Recipes are not here.** The website has recipe pages but registers no `recipe` REST route, so
  they are not reachable through this API. Say so rather than substituting a similar recipe.
- **No rate limit is published.** Keep to a polite fixed rate.

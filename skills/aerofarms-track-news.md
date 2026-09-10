---
name: aerofarms-track-news
description: >-
  Monitor AeroFarms press releases and company announcements — new product launches, retail
  distribution, research results, ownership changes — through the anonymous WordPress REST API behind
  www.aerofarms.com. Use when you need to know what AeroFarms has said publicly, and when.
api: aerofarms:aerofarms-news-api
spec: openapi/aerofarms-news-openapi.yml
operations:
  - listNewsPosts
  - getNewsPost
  - listComments
  - listCategories
  - listTags
  - searchSiteContent
generated: '2026-09-10'
method: generated
source: openapi/aerofarms-news-openapi.yml, openapi/aerofarms-taxonomy-openapi.yml, openapi/aerofarms-search-openapi.yml
---

# Track AeroFarms news

AeroFarms publishes 289 dated press items at `https://www.aerofarms.com/news/`. The same archive is
readable as JSON, anonymously, with no key and no signup.

Base URL: `https://www.aerofarms.com/wp-json`

## 1. Get the newest items

`listNewsPosts` — `GET /wp/v2/posts`

Ask for only what you need and let the server sort:

```
GET /wp/v2/posts?per_page=20&orderby=date&order=desc&_fields=id,date,slug,link,title,excerpt
```

Read `X-WP-Total` for the archive size and `X-WP-TotalPages` for how far the pagination runs. Follow
`Link: rel="next"` rather than incrementing `page` yourself.

## 2. Only what changed since your last run

Store the `modified_gmt` of the newest item you have seen, then:

```
GET /wp/v2/posts?modified_after=2026-06-01T00:00:00&orderby=modified&order=desc
```

`modified_after` catches edits to existing releases as well as new ones; `after` catches only new
publications. For a monitor, `modified_after` is the safer of the two.

## 3. Read one release in full

`getNewsPost` — `GET /wp/v2/posts/{id}`

`content.rendered` is an HTML string, not plain text. Strip tags before summarising, and do not
assume the excerpt is a fair summary — it is the first paragraph, truncated.

## 4. Narrow by topic

`listCategories` (`GET /wp/v2/categories`, 9 terms) and `listTags` (`GET /wp/v2/tags`, 455 terms)
return term ids with post counts. Filter with `?categories=<id>` or `?tags=<id>`.

If you only have a keyword, `searchSiteContent` (`GET /wp/v2/search?search=<term>`) spans posts,
pages, products and FAQ entries and returns `subtype` so you can tell which is which.

## 5. Comments

`listComments` (`GET /wp/v2/comments`) returns the 16 approved comments, including AeroFarms' own
replies. These carry commenter display names and avatar URLs. They are public on the website, but do
not bulk-harvest or republish them; quote a reply from AeroFarms if you need one, and leave the
readers alone.

## Rules for this surface

- **Read-only.** Every write method on this API is credential-gated and AeroFarms issues no
  credentials to the public. Never attempt one.
- **Check `Content-Type` before parsing.** At least one route on this host answers `200` with an HTML
  marketing page instead of JSON (`/wp/v2/media/{id}` redirects to `/media-contact-form/`). A status
  code alone is not proof you got data.
- **Match on `code`, not `message`,** when you hit an error. The envelope is
  `{code, message, data:{status}}` — see `errors/aerofarms-problem-types.yml`.
- **No published rate limit and no runtime signal.** That is a reason for restraint, not permission:
  keep to a polite fixed rate and honour the cache headers.
- **This API is incidental.** AeroFarms has made no commitment to it — it is the website's own
  content API. Re-read `GET /wp-json/` before a run if anything looks different, and expect it to
  change without notice.

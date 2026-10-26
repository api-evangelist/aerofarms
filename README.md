# AeroFarms

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AeroFarms is an indoor vertical farming company founded in 2004 and headquartered in Newark, New
Jersey. It grows leafy greens and nutrient-dense microgreens — micro broccoli, micro arugula, micro
wasabi mustard and the FlavorSpectrum blends — aeroponically, without sun or soil, under LED light
recipes driven by its proprietary agSTACK PLC/SCADA control stack. It is a Certified B Corporation
and was acquired by an affiliate of Palm Ventures in June 2026.

## What this profile found

AeroFarms is a produce grower, not a software vendor. It publishes **no developer portal, no API
documentation, no SDKs, no pricing and no API product**, and its GitHub organization
([AeroFarms](https://github.com/AeroFarms)) has zero public repositories. `api.aerofarms.com`,
`docs.aerofarms.com`, `developers.aerofarms.com` and `status.aerofarms.com` are all NXDOMAIN.

It nevertheless has a real, live, machine-readable surface, and it is more interesting than that
summary suggests:

- **A hosted MCP server.** `https://www.aerofarms.com/wp-json/mcp/mcp-oauth-server` answers JSON-RPC
  and defends itself with a textbook OAuth 2.1 challenge — RFC 9728 protected-resource metadata named
  in `WWW-Authenticate`, RFC 8414 authorization-server metadata served at the canonical path, PKCE
  S256 required, public clients via client_id metadata documents, one scope (`mcp`). `tools/list`
  returns 401, so the tool set is not enumerated here and no candidate list is invented for it.
- **An anonymous, read-only REST API** — 917 registered routes across 46 namespaces, of which the
  publicly readable part covers 289 news posts, 56 pages, 8 microgreens products, 18 FAQ answers,
  5,605 media attachments, 455 tags and a cross-content search index of 371 objects.
- **A provider-published `llms.txt`** (Yoast-generated), saved verbatim in `llms/`.

## What is in this repository

Nine OpenAPI documents in `openapi/` (25 operations) derived from the site's **own** published route
index and per-route `OPTIONS` schema documents — AeroFarms publishes no OpenAPI, and nothing in them
is invented. Alongside them: `mcp/`, `well-known/` (two real OAuth discovery documents, saved
verbatim), `authentication/`, `scopes/`, `conformance/`, `conventions/`, `errors/`, `lifecycle/`,
`data-model/`, `plans/`, `rate-limits/`, `security/`, `overlays/`, `llms/` and three packaged agent
skills in `skills/`.

Three findings a consumer should read before calling anything:

1. `/wp-json/wp/v2/media/{id}` is captured by a site redirect rule and answers **HTTP 200 with an
   HTML marketing page** instead of the attachment. Assert `Content-Type` on every response.
2. WooCommerce runs here as a catalog, not a shop: every product returns a price of `0` in an
   unconfigured `GBP` default currency. Do not read it as a price list.
3. Nothing on this surface is committed to. It exists as a by-product of running a WordPress site,
   so it can change on any plugin update, with no changelog and no channel to announce it on.

- https://www.aerofarms.com/

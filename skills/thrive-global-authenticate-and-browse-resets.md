---
name: Authenticate and browse Thrive Resets
description: >-
  Exchange a partner API key for a bearer token, then page through Thrive
  Resets (short guided videos) with locale and search filters and fetch a
  single reset by id.
api: openapi/thrive-global-partner-api-openapi.yml
operations: [authenticatePartner, getThriveResetsV3, getResetByIdV3]
generated: '2026-07-21'
method: generated
---

# Authenticate and browse Thrive Resets

1. **Authenticate** — `POST /v1/auth` (`authenticatePartner`) with your API
   key in the `x-api-key` header and a unique `userId` in the JSON body.
   Follow the userId pattern
   `{companyname}_{environment}_{actual_user_id}_{locale}` — reused User IDs
   cause cache cross-contamination. Read the JWT from
   `data.logMeWith.partner.token`; it is valid 12 hours.
2. **List resets** — `GET /v3/resets` (`getThriveResetsV3`) with
   `Authorization: Bearer <token>`. Use `page`/`limit` for pagination,
   `locale` (e.g. `en-US`, `fr-CA`) for language, and the v3 search/filter
   parameters. v1/v2 return up to 200 items without pagination.
3. **Fetch one reset** — `GET /v3/resets/{id}` (`getResetByIdV3`) with the
   UUID id.

Rules:
- Every error returns `{ message, valid: false }` as `application/json`
  (no problem+json). 401 means the token expired or is invalid — re-auth
  immediately; refresh proactively after ~11 hours.
- Collection payloads nest items at a resource-specific path (e.g.
  `data.data.reset.thrive.get.items`).
- No rate limits are enforced, but the API sits behind Cloudflare — avoid
  burst traffic.

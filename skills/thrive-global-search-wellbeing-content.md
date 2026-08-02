---
name: Search the Thrive content library
description: >-
  Authenticate and run full-text search across Thrive wellbeing content,
  handling the search-not-enabled 403 and parameter-validation 422 cases.
api: openapi/thrive-global-partner-api-openapi.yml
operations: [authenticatePartner, searchContent]
generated: '2026-07-21'
method: generated
---

# Search the Thrive content library

1. **Authenticate** — `POST /v1/auth` (`authenticatePartner`) with
   `x-api-key` header + unique `userId`; take the 12-hour JWT from
   `data.logMeWith.partner.token`.
2. **Search** — `GET /v1/search` (`searchContent`) with
   `Authorization: Bearer <token>` and your query parameters.

Rules:
- A `403` here means Thrive Search is not enabled for your account —
  surface "contact your Thrive Global representative to enable it" rather
  than retrying.
- A `422` means invalid query parameters (unknown type, missing query) —
  fix the request, do not retry as-is.
- Errors use the `{ message, valid: false }` envelope; `401` = expired or
  invalid token, re-auth and retry once.

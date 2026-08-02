---
name: Serve a daily wellbeing content mix
description: >-
  Build a localized daily wellbeing feed for a user: microsteps, stories,
  learning courses, and podcasts from the Thrive content library.
api: openapi/thrive-global-partner-api-openapi.yml
operations:
  [authenticatePartner, getDailyMicrosteps, getThriveStoriesV3,
   getLearningCourses, getThrivePodcasts]
generated: '2026-07-21'
method: generated
---

# Serve a daily wellbeing content mix

1. **Authenticate** — `POST /v1/auth` (`authenticatePartner`) with
   `x-api-key` + a `userId` unique per user AND per locale (e.g.
   `acmecorp_prod_user123_en-US`); token at `data.logMeWith.partner.token`,
   12-hour TTL.
2. **Microsteps** — `GET /v1/microsteps` (`getDailyMicrosteps`), optionally
   client-side filtered; each microstep carries its journeys (the five
   behaviors: Connection, Food, Movement, Sleep, Stress Management).
3. **Stories** — `GET /v3/stories` (`getThriveStoriesV3`) with
   `page`/`limit` and `locale`; v3 adds recipe fields.
4. **Courses** — `GET /v2/learn` (`getLearningCourses`) with pagination.
   Do not use `/v1/learn` (`getLearningCoursesV1`) — it is deprecated.
5. **Podcasts** — `GET /v2/podcasts` (`getThrivePodcasts`) returns seasons
   and episodes.

Rules:
- Pass `locale` in ISO form (`en-US`, `fr-CA`, `es-US`) and keep a distinct
  userId per locale so cached content stays locale-correct.
- Set `stripExternalLinks=true` when embedding content in surfaces that
  must not link off-Thrive domains.
- All errors use `{ message, valid: false }`; on `401` re-auth immediately.

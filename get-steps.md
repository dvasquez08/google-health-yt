CONTEXT — verify against live docs, do not rely on training memory:
This continues the fitbit-ai-agent project (TypeScript + Express +
google-auth-library), which ALREADY has a working Google OAuth 2.0 foundation
for the Google Health API (the 2026 replacement for the Fitbit Web API — NOT
Google Fit). tokens.json persistence and auto-refresh already work. Do NOT
rebuild the OAuth layer.

Docs (fetch if you can):
- Endpoints overview:  https://developers.google.com/health/endpoints
- dataPoints.list:     https://developers.google.com/health/reference/rest/v4/users.dataTypes.dataPoints/list
- dataPoints.dailyRollUp:
    https://developers.google.com/health/reference/rest/v4/users.dataTypes.dataPoints/dailyRollUp
- Data types + rules:  https://developers.google.com/health/data-types

Key facts:
- API base: https://health.googleapis.com/v4
- Daily step TOTALS come from the dailyRollUp method, NOT list. For steps, the
  list endpoint returns minute intervals with NO count value — only
  rollup / dailyRollUp return actual counts.
- countSum is a STRING — parse to int.
- Presence-aware truth: for steps, a date MISSING from the rollup means the
  device wasn't worn / didn't sync that day (render "no data"), NOT zero. A
  bucket with countSum "0" is a genuine zero.
- dailyRollUp max query range for steps is 90 days (a single day is fine).
- Auth: REUSE the existing access-token / refresh helper in health.ts. Do not
  add a new auth path.
- IMPORTANT: Confirm the exact request-body and response field names against the
  v4 discovery doc (https://health.googleapis.com/$discovery/rest?version=v4)
  before coding — do not trust paraphrased summaries.

TASK:
Implement step-count retrieval for the fitbit-ai-agent project, building on the
existing OAuth foundation. Still do NOT build the AI agent — agent.ts stays a
placeholder.

Requirements:
1. In health.ts, add getDailySteps(date: string) that:
   - takes a YYYY-MM-DD date,
   - builds the civil-day range for that date in my local timezone,
   - calls the steps dailyRollUp endpoint using the existing authenticated
     request helper,
   - parses the count (string -> number) for the requested date,
   - returns { date, steps } when present, or
     { date, steps: null, reason: "no data (device not worn or not synced)" }
     when the date is absent.
2. Also add getStepsToday() that calls getDailySteps with today's local date.
3. Add Express routes:
   - GET /steps/today            -> today's total
   - GET /steps?date=YYYY-MM-DD  -> that date's total
   Return clean JSON plus a short human-readable line.
4. Require authentication: if there's no valid token, respond with a clear
   message pointing to /auth/google instead of crashing.
5. Handle Health API errors with useful messages: 401 / invalid token, 403 /
   scope, 429 / rate limit, and empty results.
6. Do NOT hardcode any user ID — use users/me. Do NOT add write scopes.
7. Reuse the existing project style, config loading, and error handling.
8. Before coding, re-read health.ts and server.ts so you extend them.
9. After building, show me how to test, but let me make the live calls since
   they hit my real data. State the timezone assumption you used.

Do NOT implement the AI agent yet.

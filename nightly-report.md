Add a standalone nightly step-report command to the fitbit-ai-agent project.
Reuse getDailySteps and the existing auth/token handling. Don't touch the
existing routes or getDailySteps itself.

REQUIREMENTS:
1. Add src/report.ts, runnable as `npm run report`, that:
   - defaults to YESTERDAY (local Edmonton time) as the target day, since by
     evening the device has synced the full prior day; accept an optional date
     argument (YYYY-MM-DD) to override.
   - also fetches the 3 days BEFORE the target day (4 days total) via
     getDailySteps, to build a short comparison window.
   - runs headless off the stored tokens.json (no browser, no server); refreshes
     the access token via existing logic if expired.
2. Compute and report, in plain text:
   - target day's step count
   - the average of the prior 3 days (ignore days with no data — don't count a
     null as zero, since missing = not synced, not a real zero)
   - how the target day compares to that average: absolute difference and
     percentage, phrased simply (e.g. "1,240 steps above your 3-day average"
     or "18% below").
   - a tiny inline trend of the 4 days, e.g. "Aug 18: 8,102 | Aug 19: 10,313 |
     Aug 20: no data | Aug 21: 9,540".
   - if the target day itself has no data, say so clearly and skip the comparison.
3. Output to console AND append the report (with a timestamp) to
   reports/steps-log.md. Create the folder if missing; keep it gitignored.
4. If the refresh token is invalid/expired (invalid_grant), exit with a clear
   message telling me to re-authenticate at /auth/google — don't crash silently.
5. Schedule: this runs nightly at 6:00 PM Mountain time. In the README, give the
   exact Windows Task Scheduler setup (Create Task → Trigger: Daily at 6:00 PM →
   Action: run `npm run report` in the project directory), and note the machine
   must be awake at 6 PM.

After building, run `npm run report 2026-06-30` (a settled day with data before
it) to prove the full comparison works end to end, and show me the console output
and the appended log entry.

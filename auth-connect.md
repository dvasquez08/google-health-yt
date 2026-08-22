Act as a guided runner for testing the OAuth flow. Handle all the terminal
steps — starting, stopping, and restarting the dev server, builds, and checks.
I'll only do the browser consent click. Don't print .env contents or secrets
(I'm recording).

1. Confirm .env has non-empty GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET,
   GOOGLE_REDIRECT_URI, PORT — report only which are present, never values.
2. Run `npm run build`; confirm it compiles clean.
3. Start `npm run dev` in the background; curl /health and / and confirm the
   server is up and reports "not authenticated".
4. STOP and tell me to do the browser consent now (open localhost:3000, connect,
   sign in as my test-user gmail, click through the unverified-app screen, grant).
   Wait until I reply "done" — do not proceed on your own.
5. After "done": confirm tokens.json exists, curl / and confirm it now reports
   authenticated. Then restart the dev server yourself and curl / again to prove
   the session persists across a restart. Give a PASS/FAIL for each check.
6. On failure: redirect_uri_mismatch → .env vs Console mismatch; access_denied →
   account isn't a test user; invalid_grant → delete tokens.json and retry.

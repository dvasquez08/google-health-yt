CONTEXT — verify against these, do not rely on training memory:
The Google Health API is Google's 2026 replacement for the Fitbit Web API. It is
NOT Google Fit and NOT the legacy Fitbit Web API — both are being deprecated. Do
not generate Google Fit (fitness.*) or Fitbit Web API code.

Authoritative docs (fetch if you can):
- Overview:       https://developers.google.com/health/about
- Setup/OAuth:    https://developers.google.com/health/setup
- Scopes:         https://developers.google.com/health/scopes
- REST reference: https://developers.google.com/health/reference/rest/v4

Key facts:
- OAuth authorize endpoint: https://accounts.google.com/o/oauth2/v2/auth
- Token endpoint:           https://oauth2.googleapis.com/token
- API base URL:             https://health.googleapis.com/v4/
- Scope for this project:   https://www.googleapis.com/auth/googlehealth.activity_and_fitness.readonly
- Use access_type=offline to receive a refresh token.
- Do NOT set include_granted_scopes, and avoid prompt=consent unless a new refresh token is needed — mixing legacy Google Fit scopes breaks the Health API auth layer.
- Use google-auth-library (OAuth2Client) for the flow.

TASK:
I want you to set up a new TypeScript Node.js project called "fitbit-ai-agent".
The goal is to connect my personal Fitbit data through Google's new Google Health API and eventually expose that data to an AI agent. (Data source = Fitbit; API = Google Health API.)
For now, ONLY build the project foundation and Google OAuth integration. Do not implement the AI agent yet.

Requirements:
1. Create a clean TypeScript Node.js project.
2. Use Express for the local web server.
3. Use dotenv for environment variables.
4. Use Google's official Node.js libraries where appropriate (google-auth-library for OAuth).
5. Create a clear project structure such as:
   src/
     server.ts
     auth.ts
     health.ts
     agent.ts
6. Implement Google OAuth 2.0 for the Google Health API using OAuth2Client. When building the authorization URL, set access_type=offline so a refresh token is returned. Do NOT set include_granted_scopes. Only add prompt=consent when a new refresh token is explicitly needed.
7. The OAuth scope should initially be exactly:
   https://www.googleapis.com/auth/googlehealth.activity_and_fitness.readonly
8. Create routes for:
   GET /               - landing page showing whether the user is authenticated
   GET /auth/google    - starts Google OAuth
   GET /auth/callback  - handles the OAuth callback (exchange code, persist tokens)
   GET /health         - confirms the server is running
9. Read these values from .env:
   GOOGLE_CLIENT_ID
   GOOGLE_CLIENT_SECRET
   GOOGLE_REDIRECT_URI
   PORT
10. Do NOT hard-code credentials or tokens anywhere.
11. Add a .gitignore that excludes: .env, node_modules, dist, tokens.json
12. Create a .env.example with the required env vars but no real credentials. GOOGLE_REDIRECT_URI should default to http://localhost:3000/auth/callback (matching PORT).
13. Store the OAuth refresh token securely for this local dev project. Use a simple approach: persist the token set to a gitignored tokens.json file (no database). On startup, load tokens.json if present so the app remembers the session, and refresh the access token automatically when it expires.
14. Add useful error handling around the OAuth flow (missing/expired tokens, denied consent, redirect_uri mismatch, token exchange failures) with clear log messages.
15. Add npm scripts for: npm run dev, npm run build, npm run start
16. Create a README.md explaining: prerequisites (Node version; a PERSONAL Google account with Fitbit linked — Workspace accounts are not supported); how to install dependencies; how to configure Google Cloud (enable the Google Health API; OAuth consent screen in Testing with your account as a test user; create a Web application OAuth client; add http://localhost:3000/auth/callback as an Authorized redirect URI); where to put OAuth credentials (.env); how to start the dev server; how the OAuth flow works.
17. Before writing code, inspect the current project directory and existing files so you don't overwrite anything important.
18. Use current Google Health API documentation and current Google OAuth practices (see CONTEXT above). Do NOT use the deprecated Fitbit Web API or Google Fit API.

After creating the project, explain: what files you created, what dependencies you installed, how I should configure Google Cloud, and how I should test the OAuth flow.

Do not implement any Fitbit/Health API data retrieval yet. We will do that in the next step.

PERSONAL CA — BUTTONS FIX + RENDER DEPLOYMENT

WHAT WAS FIXED
1. The saved server added a Content-Security-Policy that blocked all inline JavaScript. This website uses inline <script> blocks and onclick="..." handlers, so the browser blocked the code that makes buttons respond. The CSP now permits the existing inline handlers.
2. The API now defaults to the same Render origin, rather than an old hard-coded Render hostname.
3. The service-worker cache was bumped to v2 so older cached shell files are retired after update.
4. Added the missing module-level `re` import used by the PWA asset route.
5. HTML responses use no-store to reduce stale-page issues after deployment.

DEPLOY ON RENDER (SAME SERVICE)
1. Download and extract this ZIP.
2. In the GitHub repository connected to Render, replace these three root files from this ZIP: `index.html`, `server.py`, and `service-worker.js`. Keep your other existing project files and folders. Do not upload any real secrets or .env file to GitHub.
3. Commit the changes to the same branch Render deploys.
4. In Render, confirm Build Command: pip install -r requirements.txt
5. Confirm Start Command: uvicorn server:app --host 0.0.0.0 --port $PORT
6. Let the deployment finish, then open your Render URL in Chrome and refresh.

IMPORTANT ENVIRONMENT VARIABLES
- GROQ_API_KEY: keep the real key only in Render Environment.
- GROQ_MODEL: openai/gpt-oss-20b
- SESSION_SECRET: keep the already-generated value in Render. Do not replace it on every deploy.
- DATA_ENCRYPTION_KEY: keep the already-generated Fernet key in Render. Do not replace it; replacing it can prevent reading data encrypted with the old key.
- FRONTEND_ORIGIN: leave empty for this single-origin Render deployment.
- GOOGLE_CLIENT_ID: optional, only needed for Google Sign-In.

TEST AFTER DEPLOYMENT
Open these paths under your Render domain: /api/health, /api/config, /assets/personal-ca-icon.png, /manifest.webmanifest. Each should load. Then test Home, Tax Assistant, Sign In, Stocks AI, Rules, Updates, and RDP. AI chat still depends on a valid Groq key and enabled model.

SECURITY NOTE
This quick repair allows inline scripts because the current HTML contains inline JavaScript and inline onclick handlers. A stronger future approach is moving all scripts and handlers into external JavaScript and then removing 'unsafe-inline' from the policy. Email-only sign-in in this project is a lightweight project session, not proof that the person owns that email.

RENDER SERVICE NAME / FREE URL
A Render web service has an onrender.com hostname based on its service name. Renaming the service can change its onrender.com URL; the desired name must be available. Update saved links and any external integrations after renaming.


PWA INSTALL + LOGO FIX
- Added dedicated clean launcher icons in 192x192 and 512x512 PNG sizes.
- Manifest now declares standard Android PWA icon sizes and a maskable icon.
- Added an always-visible Install App button. It uses Chrome's install prompt when available; otherwise it explains the Chrome menu fallback.
- Bumped service-worker cache to v3 and precaches the new launcher icons.
- The site must be served over HTTPS (Render's onrender.com URL is HTTPS).
- After deploying, open /manifest.webmanifest and both /assets/personal-ca-icon-192.png and /assets/personal-ca-icon-512.png. They must load.
- On Android Chrome, open the site, refresh, then tap Install App. If the prompt is not available, use Chrome ⋮ > Install app or Add to Home screen. PWA install prompts are controlled by Chrome and cannot be forcibly displayed by a webpage.

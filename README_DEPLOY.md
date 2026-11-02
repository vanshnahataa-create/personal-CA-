# Personal CA — Render-only deployment (Groq AI)

## Render Web Service
1. Keep `server.py`, `index.html`, `requirements.txt`, `legal_knowledge/`, `current_updates.json`, `assets/`, `manifest.webmanifest`, and `service-worker.js` together in the repository root.
2. In Render, open the existing Python Web Service connected to this GitHub repository.
3. Root Directory: `.`
4. Build Command: `pip install -r requirements.txt`
5. Start Command: `uvicorn server:app --host 0.0.0.0 --port $PORT`
6. Set the environment variables below in Render Dashboard → Environment. Do not put real keys in GitHub.

## Environment variables
- `GROQ_API_KEY`: private key from Groq Console.
- `GROQ_MODEL`: `openai/gpt-oss-20b`.
- `SESSION_SECRET`: long random string of at least 32 characters; keep the existing value stable.
- `DATA_ENCRYPTION_KEY`: valid Fernet key; keep the existing value stable. Changing it can make previously encrypted data unreadable.
- `FRONTEND_ORIGIN`: leave empty for this same-origin Render deployment.
- `GOOGLE_CLIENT_ID`: optional; only needed for Google Sign-In.
- Render sets `RENDER=true`; no separate `NODE_ENV` is required.

## Test after deployment
- `/api/health` — backend status and AI configuration.
- `/api/config` — public sign-in configuration.
- `/assets/personal-ca-icon.png` — app icon.
- `/manifest.webmanifest` — PWA manifest.
- `/service-worker.js` — updated service worker.

The site and API share one Render origin. The frontend therefore calls `location.origin`; do not point it at the old `personal-ca-1.onrender.com` service.

Groq availability, rate limits, and model access depend on the user's Groq account. Email-only sign-in is a lightweight project session, not proof of email ownership. Stocks AI provides educational information and does not use a live market-data feed. Verify tax and legal outputs against current official sources.

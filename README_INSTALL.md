# Personal CA — Render + PWA installation

## Render
Build command: `pip install -r requirements.txt`
Start command: `uvicorn server:app --host 0.0.0.0 --port $PORT`

Keep secrets (GROQ_API_KEY, SESSION_SECRET, DATA_ENCRYPTION_KEY, GOOGLE_CLIENT_ID) in Render Environment only.
Set FRONTEND_ORIGIN to the exact deployed frontend origin if hosting the frontend separately.

## Installation
Deploy on HTTPS. Open the website in Chrome on Android, then use Chrome menu → **Install app** or **Add to Home screen**. The browser decides when to show its install prompt; it cannot be forced to appear on every visit. The app icon and manifest are included. The service worker caches only the static shell and never caches `/api/` requests.

## Important
This package is syntax-checked, but real sign-in, Groq API access, and installation must be tested after deployment with your own environment variables.

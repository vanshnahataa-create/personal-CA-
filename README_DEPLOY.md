# Personal CA — Render deployment (Groq AI)

## Render Web Service
1. Upload the extracted files to the root of your GitHub repository. Keep `server.py`, `index.html`, `requirements.txt`, `legal_knowledge/`, `current_updates.json`, `assets/`, and the other included files together.
2. In Render, create/open a **Python Web Service** connected to this repository.
3. Root Directory: `.`
4. Build Command: `pip install -r requirements.txt`
5. Start Command: `uvicorn server:app --host 0.0.0.0 --port $PORT`
6. Add Environment Variables listed below, then deploy.

## Required Render environment variables
- `GROQ_API_KEY`: create a key in the Groq Console. Keep it only in Render; never put it in `index.html` or GitHub.
- `GROQ_MODEL`: `llama-3.3-70b-versatile` (change only to a model currently enabled in your Groq account).
- `SESSION_SECRET`: a long random string, at least 32 characters.
- `DATA_ENCRYPTION_KEY`: a valid Fernet key. Generate once with Python: `from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())`. Keep it stable; do not regenerate on each deploy.
- `FRONTEND_ORIGIN`: if using external Netlify/Vercel frontend, set its exact origin without a final slash, e.g. `https://your-site.vercel.app`. Multiple comma-separated origins are supported. If the frontend is served from this Render service, this can be empty.
- `GOOGLE_CLIENT_ID`: optional; only needed for Google sign-in.
- `NODE_ENV=production` (Render is also detected automatically).

## URLs to test after deployment
- `/api/health` — service status and `aiConfigured` / provider.
- `/api/` — API root.
- `/` — website frontend if hosted on this same Render service.

## If hosting frontend separately
The HTML automatically calls `https://personal-ca-1.onrender.com` when it is not served from an `onrender.com` hostname. Update `FRONTEND_ORIGIN` to the actual Vercel/Netlify domain and redeploy Render. If your Render service has a different URL, edit `window.PERSONAL_CA_API` in `index.html` to that backend URL.

## Important notes
- Groq API access is subject to Groq account/model availability, rate limits, and current plan limits; this package does not guarantee unlimited/free inference.
- Email-only login in this project does not verify email ownership and is not suitable as production-grade authentication.
- Stocks AI is educational only and does not provide live market quotes or personalized trading instructions. Tax/legal outputs should be checked against official current sources.

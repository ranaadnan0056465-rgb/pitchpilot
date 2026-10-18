PitchPilot — fixed project package
=================================

Design preservation
-------------------
Existing HTML pages and their visual styling were preserved. The only frontend
edits are response/error parsing in generator.html and script.js. The API URL
remains /.netlify/functions/generate.

Files added
-----------
netlify/functions/generate.js  Backend Netlify Function
netlify.toml                   Netlify publish/functions configuration

Before deploying
----------------
1. In Netlify Site configuration > Environment variables, set:
   GROQ_API_KEY = your Groq API key (secret; never put it in frontend files)
   ALLOWED_ORIGINS = https://YOUR-SITE-NAME.netlify.app
   You may include a custom domain too, comma-separated.
   GROQ_TIMEOUT_MS = 18000 (optional)
   RATE_LIMIT_WINDOW_MS = 60000 (optional)
   RATE_LIMIT_MAX = 8 (optional)
2. Confirm the two configured models are available for your Groq account:
   openai/gpt-oss-120b
   openai/gpt-oss-20b
   If not, replace them in MODEL_ALLOWLIST with supported model IDs for your account.
3. Deploy the full contents of this folder to Netlify, not only the HTML files.
4. Test the generator with a normal job description and skills.

Important limitations
---------------------
The in-memory rate limiter is best-effort per warm serverless instance; it is
not a global limit across all instances. For enforceable global quotas, use a
shared store such as Redis/Upstash. The backend has not been tested against
your live Groq key from this environment. API credentials are intentionally
not included in this package.

Success response stays compatible with the current frontend:
{ "hook": "...", "body": "...", "cta": "..." }

Errors use a structured response:
{ "error": { "code": "...", "message": "..." } }

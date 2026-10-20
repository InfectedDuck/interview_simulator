# Setup and Deployment

## Prerequisites
- Python 3.11+
- Node.js 18+
- Ollama with Mistral model

## Installation

```bash
# 1. Pull the LLM model
ollama pull mistral:7b-instruct-q4_0

# 2. Backend
cd backend
pip install -r requirements.txt
python run.py                      # Starts on http://localhost:8000

# 3. Frontend (new terminal)
cd frontend
npm install
npm run dev                        # Starts on http://localhost:5173
```

The Whisper speech model (~150MB) downloads automatically on first voice recording.

## Configuration

All settings configurable via environment variables (prefix `INTERVIEW_`):

| Variable | Default | Description |
|----------|---------|-------------|
| `INTERVIEW_LLM_PROVIDER` | `ollama` | LLM provider: `ollama` (local) or `openrouter` (cloud) |
| `INTERVIEW_OLLAMA_MODEL` | `mistral:7b-instruct-q4_0` | Ollama model name |
| `INTERVIEW_OPENROUTER_API_KEY` | *(empty)* | OpenRouter API key ([get one free](https://openrouter.ai/keys)) |
| `INTERVIEW_OPENROUTER_MODEL` | `nvidia/nemotron-nano-9b-v2:free` | OpenRouter model (free tier) |
| `INTERVIEW_WHISPER_MODEL` | `base` | Whisper model (tiny/base/small/medium) |
| `INTERVIEW_WHISPER_LANGUAGE` | `en` | Speech recognition language |
| `INTERVIEW_MAX_QUESTIONS_PER_SESSION` | `5` | Base questions per interview |
| `INTERVIEW_CORS_ORIGINS` | `["http://localhost:5173"]` | Allowed frontend origins |

---

## Cloud Deployment

The app deploys as two services: **frontend on Vercel** (free) + **backend on Render** (free).

### Step 1: Push to GitHub

```bash
git add -A && git commit -m "Prepare for deployment"
git remote add origin https://github.com/YOUR_USERNAME/interview-tester.git
git push -u origin main
```

### Step 2: Deploy Backend on Render

1. Go to [render.com](https://render.com) and sign up (free)
2. Click **New > Web Service** and connect your GitHub repo
3. Configure:
   - **Root directory:** `backend`
   - **Build command:** `pip install -r requirements.txt`
   - **Start command:** `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
4. Add environment variables:
   | Key | Value |
   |-----|-------|
   | `INTERVIEW_LLM_PROVIDER` | `openrouter` |
   | `INTERVIEW_OPENROUTER_API_KEY` | `sk-or-...` (your key from [openrouter.ai/keys](https://openrouter.ai/keys)) |
   | `INTERVIEW_OPENROUTER_MODEL` | `nvidia/nemotron-nano-9b-v2:free` |
   | `INTERVIEW_CORS_ORIGINS` | `["https://YOUR-APP.vercel.app"]` |
   | `INTERVIEW_WHISPER_MODEL` | `tiny` |
5. Click **Create Web Service** — note the URL (e.g., `https://interview-tester-api.onrender.com`)

### Step 3: Deploy Frontend on Vercel

1. Go to [vercel.com](https://vercel.com) and import your GitHub repo
2. Configure:
   - **Root directory:** `frontend`
   - **Framework preset:** Vite
3. Add environment variable:
   | Key | Value |
   |-----|-------|
   | `VITE_API_URL` | `https://interview-tester-api.onrender.com` |
4. Click **Deploy**

### Free Tier Notes

- **Render free tier** spins down after 15 min of inactivity. First request takes ~30s to wake up. This is normal for a demo.
- **OpenRouter free models** have rate limits (~10 req/min). Fine for normal use, may throttle rapid clicking.
- **SQLite on Render free tier** is ephemeral — data resets on each deploy. For persistent data, upgrade to Render paid ($7/mo) or switch to Supabase PostgreSQL.

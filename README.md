# Interview Tester

**Practice university admission interviews with an AI interviewer that adapts to your answers.** Built for Hong Kong universities, with a detailed knowledge base for CUHK.

**Live demo:** [cuhkinterviewsimulator.vercel.app](https://cuhkinterviewsimulator.vercel.app/). It runs on free hosting and a free AI model, so the first load can take ~30 seconds and replies 15–60 seconds. For the full experience, [run it locally](docs/SETUP.md).

Students upload their CV and essays, record a spoken introduction, and then sit a realistic interview. The AI interviewer asks about their own projects, follows up on weak answers, and gives specific feedback after each answer, including a rewritten version of the answer.

## Why

Admission interviews are high-stakes, and most students prepare by memorizing answers. Private interview coaches in Hong Kong charge HK$1,500+ an hour. This project tries to give every student the core of what a coach does: know the student, hold a real conversation, and give specific feedback.

## What it does

- **Builds a profile** from a voice introduction, CV and personal statement (PDF/DOCX), merging new information instead of overwriting it
- **Runs a real conversation**, not a fixed quiz: the next question depends on how well you answered the last one
- **Knows CUHK**: core values and 9 programmes, used to ask and score programme-specific questions
- **Scores each answer** on up to 6 dimensions (content, relevance, clarity, values alignment, time management…)
- **Shows a better answer**: your answer rewritten side by side with what was improved
- **Two modes**: untimed practice, or a timed simulation with a 2-minute countdown and spoken questions
- **Tracks progress** across sessions, and lets you re-answer a question to see your score change
- **Transcribes speech** locally with Whisper, then corrects names and technical terms using your profile

Details for each feature: [docs/FEATURES.md](docs/FEATURES.md)

## How it works

```
React (Vite) ──audio/JSON──▶ FastAPI ──▶ LLM (Ollama locally, or OpenRouter in the cloud)
                               │
                               ├──▶ faster-whisper (speech-to-text)
                               └──▶ SQLite (profiles, sessions, analytics)
```

One `generate(prompt, system)` function sits in front of every LLM call, so switching between a local model and a cloud model is a single environment variable. Architecture, API endpoints and database schema: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React 19, Vite, React Router, Recharts |
| Backend | FastAPI, async Python (aiosqlite, httpx) |
| AI | Ollama (Mistral 7B) locally, or OpenRouter free models |
| Speech | faster-whisper |
| Documents | pdfplumber, python-docx |

## Run locally

```bash
ollama pull mistral:7b-instruct-q4_0

cd backend && pip install -r requirements.txt && python run.py   # http://localhost:8000
cd frontend && npm install && npm run dev                         # http://localhost:5173
```

Configuration and cloud deployment (Render + Vercel): [docs/SETUP.md](docs/SETUP.md)

## License

[MIT](LICENSE)

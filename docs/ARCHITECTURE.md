# Architecture

```
+------------------+         +------------------+         +------------------+
|                  |  Audio  |                  |  HTTP   |                  |
|   React Frontend |-------->|  FastAPI Backend  |-------->|   Ollama (LLM)   |
|   (Vite + JSX)   |<--------|  (Python 3.14)   |<--------|   Mistral 7B     |
|                  |  JSON   |                  |         |                  |
+------------------+         +--------+---------+         +------------------+
                                      |
                              +-------+-------+
                              |               |
                        +-----+-----+   +-----+-----+
                        |  SQLite   |   |  Whisper   |
                        |  Database |   |  STT (CPU) |
                        +-----------+   +-----------+
```

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | React 19 + Vite | Interactive web UI with voice recording |
| Backend | FastAPI + Python 3.14 | API server, AI orchestration, document parsing |
| LLM | Ollama (local) or OpenRouter (cloud) | Question generation, answer analysis, coaching |
| Speech-to-Text | faster-whisper (base) | Local audio transcription with AI correction |
| Database | SQLite + aiosqlite | Session history, profiles, analytics |
| Document Parser | pdfplumber + python-docx | CV/essay text extraction |

**Dual mode:** Runs fully local with Ollama (no API keys, no data leaves the machine), or deploys to the cloud with OpenRouter using free models.

---

## Project Structure

```
interview_tester/
├── backend/
│   ├── app/
│   │   ├── main.py                          # FastAPI app + CORS + health check
│   │   ├── config.py                        # Environment-based settings
│   │   ├── database.py                      # SQLite schema (7 tables) + migrations
│   │   ├── models/
│   │   │   ├── user.py                      # User + Profile models
│   │   │   ├── interview.py                 # Interview request/response models
│   │   │   └── session.py                   # Session + Exchange models
│   │   ├── routers/
│   │   │   ├── users.py                     # User CRUD
│   │   │   ├── profile.py                   # Profile extraction + transcription
│   │   │   ├── interview.py                 # Interview flow + coaching + replay
│   │   │   ├── sessions.py                  # Session history queries
│   │   │   ├── briefing.py                  # Strategy briefing generation
│   │   │   └── analytics.py                 # Performance analytics
│   │   └── services/
│   │       ├── ollama.py                    # LLM interface (Ollama chat API)
│   │       ├── whisper_stt.py               # Speech-to-text + AI correction
│   │       ├── conversation_engine.py       # Adaptive question generation
│   │       ├── response_analyzer.py         # Multi-dimensional scoring
│   │       ├── answer_coach.py              # Answer improvement with STAR
│   │       ├── profile_extractor.py         # AI profile parsing + merging
│   │       ├── strategy_briefing.py         # Pre-interview coaching reports
│   │       ├── analytics.py                 # Progress tracking + pattern detection
│   │       ├── cuhk_knowledge.py            # CUHK intelligence (9 programs)
│   │       └── document_parser.py           # PDF/DOCX/TXT parsing
│   ├── requirements.txt
│   └── run.py
│
└── frontend/
    ├── src/
    │   ├── App.jsx                          # Router (8 routes)
    │   ├── api/client.js                    # Backend API wrapper
    │   ├── hooks/
    │   │   ├── useAudioRecorder.js          # MediaRecorder API wrapper
    │   │   └── useSpeechSynthesis.js        # Browser TTS wrapper
    │   ├── pages/
    │   │   ├── HomePage.jsx                 # User selection + health status
    │   │   ├── ProfilePage.jsx              # Voice + document profile building
    │   │   ├── InterviewPage.jsx            # Full interview flow (practice + sim)
    │   │   ├── BriefingPage.jsx             # Pre-interview strategy report
    │   │   ├── HistoryPage.jsx              # Session history list
    │   │   ├── ResultsPage.jsx              # Session results + breakdown
    │   │   ├── AnalyticsPage.jsx            # Charts + trends + patterns
    │   │   └── ReplayPage.jsx               # Re-answer with delta scoring
    │   └── components/
    │       ├── VoiceRecorder.jsx             # Audio recording + Whisper + text fallback
    │       ├── QuestionCard.jsx              # Question display + type badges + TTS
    │       ├── FeedbackPanel.jsx             # Score visualization (up to 6 dimensions)
    │       ├── ScoreBoard.jsx                # Session completion summary
    │       ├── AnswerComparisonPanel.jsx      # Side-by-side answer improvement
    │       ├── InterviewTimer.jsx            # Countdown timer (simulation mode)
    │       ├── InterviewFormatBriefing.jsx    # Interview format info card
    │       ├── UniversitySelector.jsx         # HK university dropdown
    │       ├── CUHKProgramSelector.jsx        # CUHK program/faculty dropdown
    │       ├── DocumentUpload.jsx             # File upload for CV/essays
    │       ├── ProfileCard.jsx               # Structured profile display
    │       ├── InterviewReport.jsx           # Print-optimized PDF report layout
    │       └── ThemeToggle.jsx               # Dark/light mode toggle
    ├── index.html
    ├── vite.config.js
    └── package.json
```

---

## API Endpoints

| Method | Endpoint | Purpose |
|--------|---------|---------|
| `GET` | `/api/health` | System health + Ollama status |
| `POST` | `/api/users` | Create user |
| `GET` | `/api/users` | List users |
| `GET` | `/api/users/{id}` | Get user details |
| `POST` | `/api/transcribe` | Audio -> text (Whisper + AI correction) |
| `POST` | `/api/profile/extract` | Voice transcript -> structured profile |
| `POST` | `/api/profile/upload` | Document upload -> profile extraction |
| `GET` | `/api/profile/{id}` | Get user's profile |
| `PUT` | `/api/profile/{id}` | Update profile fields |
| `POST` | `/api/interview/start` | Start interview session |
| `POST` | `/api/interview/{id}/answer` | Submit answer, get scores + next question |
| `POST` | `/api/interview/{id}/exchange/{eid}/improve` | Generate improved answer |
| `POST` | `/api/interview/{id}/replay/{eid}` | Re-answer with delta scoring |
| `POST` | `/api/interview/{id}/end` | End session early |
| `GET` | `/api/sessions/{uid}` | List user's sessions |
| `GET` | `/api/sessions/{uid}/{sid}` | Full session with all exchanges |
| `POST` | `/api/briefing/{uid}` | Generate strategy briefing |
| `GET` | `/api/briefing/format` | Get interview format info |
| `GET` | `/api/briefing/programs` | List CUHK programs |
| `GET` | `/api/analytics/{uid}` | Full performance analytics |

---

## Database Schema

```
users ──────────< profiles (1:1)
  │
  └──────────────< sessions ──────< exchanges ──────< exchange_retries
                      │
                      └── analytics_cache
                      └── briefings
```

**7 tables** tracking users, profiles (10+ extracted fields), interview sessions with mode/program, individual Q&A exchanges with 6 scoring dimensions, answer retries for replay mode, cached analytics, and strategy briefings.

---

## Technical Highlights

**Provider-Agnostic LLM Layer:** A single `generate(prompt, system)` function serves all 10 call sites across 9 services. Switching between Ollama (local) and OpenRouter (cloud) requires changing one environment variable — zero code changes.

**Prompt Engineering:** Every AI interaction uses carefully crafted prompts with structured output parsing, retry logic for malformed JSON, and context injection from CUHK knowledge base + student profile.

**Async Architecture:** All LLM calls, database operations, and Whisper transcription are fully async using `asyncio`, `aiosqlite`, and `httpx.AsyncClient`. Whisper runs in a thread pool executor to avoid blocking the event loop.

**Adaptive Algorithms:** The conversation engine uses a strategy classifier that considers answer scores, question position, interview progress, and remaining questions to determine the optimal next question type — implementing a state machine over the interview conversation.

**Profile Merging:** Multiple document uploads don't overwrite — the AI intelligently merges new information with existing data, combining skill lists, adding new projects, and keeping the more detailed version of each field.

**Local-First with Cloud Option:** Runs fully local with Ollama for privacy, or deploys to the cloud with OpenRouter. The same codebase supports both modes via environment configuration.

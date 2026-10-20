# Features

## 1. Intelligent Profile Building

Students can build their profile through multiple channels — the system merges everything intelligently:

| Input Method | What It Extracts |
|-------------|-----------------|
| Voice Introduction | Education, skills, goals, target universities |
| CV/Resume Upload (PDF/DOCX) | Projects, experience, achievements, technologies |
| Personal Statement | Personality traits, interests, motivations |
| Essay Upload | Writing style, intellectual interests, values |

The AI extracts structured data from unstructured input:

```
Uploaded CV  ──>  AI Profile Extraction  ──>  Structured Profile
                                               ├── Education (level, institution, major, GPA)
                                               ├── Skills ["Python", "React", "Data Analysis"]
                                               ├── Experience [{title, org, description}]
                                               ├── Projects [{name, description, technologies,
                                               │              role, outcome}]
                                               ├── Goals
                                               ├── Achievements
                                               ├── Interests
                                               └── Personality Traits
```

Each upload enriches the existing profile — uploading a CV then a personal statement combines information from both without losing anything.

---

## 2. Conversational Interview Engine

This is not a quiz with 5 random questions. The AI conducts a real conversation:

```
Question Strategy Flow:

  Q1 [WARMUP]     "Tell me about yourself and what brings you to CUHK."
       │
       ▼ Student answers well (score: 8.2/10)
       │
  Q2 [CHALLENGE]   "You mentioned your event management system project.
       │            What was the most difficult technical decision you made,
       │            and would you make the same choice today?"
       │
       ▼ Student gives vague answer (score: 4.5/10)
       │
  Q3 [FOLLOW-UP]   "I'd love to hear more specifics — can you walk me through
       │            one concrete example of a trade-off you faced in that project?"
       │
       ▼ Student elaborates well (score: 7.8/10)
       │
  Q4 [CHALLENGE]   "How do you think the skills you developed in that project
       │            connect to CUHK's emphasis on social responsibility?"
       │
       ▼ ...continues adapting...
       │
  Q5 [CLOSING]     "If you could contribute one thing to the CUHK community,
                    what would it be?"
```

**How it adapts:**

| Previous Score | Strategy | Behavior |
|---------------|----------|----------|
| < 5.0 | Follow-up | Gives the student another chance to elaborate, references their specific answer |
| 5.0 - 7.0 | Probe | Asks for a concrete example or deeper reflection |
| > 7.0 | Challenge | Advances to a harder topic, tests critical thinking |
| Final question | Closing | Forward-looking, memorable, gives student a strong finish |

**Dynamic session length:** If a student struggles badly (avg < 4.5), the system automatically adds follow-up questions (up to 8 total) to give them a fair chance.

---

## 3. CUHK Deep Knowledge System

The platform contains a structured knowledge base covering 9 CUHK programs with insider-level intelligence:

```
CUHK Knowledge Base
├── Core Values
│   ├── Whole Person Development (博文約禮)
│   ├── Bilingualism (Chinese + English)
│   ├── Social Responsibility
│   └── Tradition & Innovation
│
├── 9 Colleges
│   ├── Chung Chi (Christian values, service)
│   ├── New Asia (Chinese heritage, philosophy)
│   ├── United (Social engagement)
│   ├── Shaw (Innovation, entrepreneurship)
│   └── ... 5 more
│
└── 9 Programs with Deep Insight
    ├── Medicine
    │   ├── Format: MMI (6-8 stations, 8 min each)
    │   ├── Competencies: empathy, ethical reasoning, teamwork...
    │   ├── Red Flags: memorized answers, lack of self-awareness
    │   └── Insider Tips: "Think aloud — they want your reasoning
    │                      process, not just your conclusion"
    ├── Business Administration
    │   ├── Format: Individual interview + group discussion
    │   └── ...
    ├── Law
    ├── Engineering
    ├── Science
    ├── Social Science
    ├── Arts
    ├── Education
    └── General / Undecided
```

This knowledge is injected into:
- **Question generation** — questions match the program's interview style
- **Response scoring** — answers are evaluated against program-specific rubrics
- **Strategy briefings** — advice is tailored to what each faculty actually looks for

---

## 4. Multi-Dimensional Scoring

Every answer is scored across up to 6 dimensions:

```
                    Core Scores              CUHK-Specific         Simulation
                ┌───────────────────┐   ┌──────────────────┐   ┌──────────────┐
                │  Content    8/10  │   │  Values      7/10│   │  Time    9/10│
                │  Relevance  7/10  │   │  Alignment       │   │  Management  │
                │  Clarity    9/10  │   │                  │   │              │
                │                   │   │  Self-       6/10│   │              │
                │                   │   │  Awareness       │   │              │
                └───────────────────┘   └──────────────────┘   └──────────────┘
                   Always scored           When CUHK +            Simulation
                                           program selected       mode only
```

---

## 5. Answer Improvement Coach

After each answer, students can click **"Show Me a Better Answer"** to see their answer professionally rewritten:

```
┌─────────────────────────────┐  ┌─────────────────────────────┐
│      YOUR ANSWER            │  │    IMPROVED VERSION          │
│                             │  │                              │
│ "I did a project about      │  │ "I built an event management │
│  events. It was hard but    │  │  system that handles 500+    │
│  I learned a lot."          │  │  concurrent users. The       │
│                             │  │  biggest challenge was       │
│                             │  │  designing the real-time     │
│                             │  │  notification system — I     │
│                             │  │  chose WebSockets over       │
│                             │  │  polling after benchmarking  │
│                             │  │  both approaches..."         │
└─────────────────────────────┘  └─────────────────────────────┘

Key Improvements:
  1. Added specific numbers (500+ users)
  2. Named the technical challenge
  3. Showed decision-making process
  4. Connected to concrete outcome

STAR Breakdown:
  Situation: Building event management for university
  Task:      Handle real-time updates at scale
  Action:    Benchmarked WebSockets vs polling, chose WS
  Result:    System handles 500+ concurrent users
```

The coach keeps the student's authentic stories but restructures for impact and adds specificity from their profile data.

---

## 6. Two Interview Modes

| Feature | Practice Mode | Simulation Mode |
|---------|:------------:|:--------------:|
| Timer | None | 2-minute countdown |
| Feedback | After each answer | After each answer |
| Question reading | Manual button | Auto-read via TTS |
| Time pressure scoring | No | Yes |
| Visual timer | No | Green -> Yellow -> Red |
| Auto-submit on timeout | No | Yes |

---

## 7. Pre-Interview Strategy Briefing

Before starting an interview, students can generate a personalized coaching report:

```
Strategy Briefing for: CUHK Medicine

Likely Questions:
  1. "Why medicine and not another helping profession?"
     Why they ask: Tests genuine motivation vs. parental pressure
     Your angle: Reference your hospital volunteering from your CV

  2. "Describe an ethical dilemma you've encountered"
     Why they ask: MMI stations test ethical reasoning
     Your angle: Use your community service experience as a framework

Key Talking Points:
  - Your event management project → demonstrates problem-solving under pressure
  - Hospital volunteering → shows genuine compassion through ACTION
  - DSE results → evidence of academic rigor

Do:                              Don't:
  + Use specific examples          - Memorize scripted answers
  + Show your reasoning process    - Claim to have all the answers
  + Reference CUHK's values        - Focus only on prestige
  + Acknowledge complexity          - Be dismissive of other perspectives

Opening Strategy: Start with a warm greeting. Mention something specific
about CUHK Medicine that excites you (e.g., the Problem-Based Learning
curriculum or the rural health outreach program).
```

---

## 8. Cross-Session Analytics

After completing multiple sessions, students access a performance dashboard:

```
Sessions: 5          Avg Content: 7.2     Avg Relevance: 6.8     Avg Clarity: 7.5

Improvement Velocity:
  Content:    +18.3%    ████████████████████   (strong improvement)
  Relevance:  +12.1%    █████████████          (improving)
  Clarity:    +24.7%    ████████████████████████████ (excellent growth)

Score Trends:                          Skill Breakdown:
  10│         ╱─────                        Content
   8│    ╱───╱                             ╱      ╲
   6│───╱                          Clarity ╱────────╲ Relevance
   4│                                      ╲        ╱
   2│                                       ╲──────╱
    └──────────────
     S1  S2  S3  S4  S5

Strengths:                             Areas for Growth:
  + Clear communication style            - Lacks specific examples when
  + Strong project narratives              discussing extracurriculars
  + Good self-awareness                  - Tends to give short answers
                                           under time pressure
```

---

## 9. Answer Replay & Delta Scoring

Students can revisit any past question, re-record their answer, and see exactly how they improved:

```
Original Score:  Content 5  →  New Score: Content 8  (+3.0)
                 Relevance 4              Relevance 7  (+3.0)
                 Clarity 6                Clarity 8    (+2.0)
```

This creates a deliberate practice loop: perform -> get feedback -> understand what better looks like -> retry -> measure improvement.

---

## 10. Context-Aware Speech Recognition

Standard speech-to-text makes errors like "producer" instead of "produced" or mangles proper nouns. Our system adds an AI correction layer:

```
Audio Input
    │
    ▼
Whisper STT ──> Raw: "I producer a building event management system using reacts"
    │
    ▼
AI Correction ──> Context: Student's profile has project "Building Event
    │              Management System" using ["React", "Node.js"]
    │
    ▼
Corrected: "I produced a building event management system using React"
```

The AI uses the student's profile (name, projects, skills, technologies) as context to fix mishearings.

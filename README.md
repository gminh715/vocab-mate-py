# 📚 Vocab Mate

> **Master English vocabulary in authentic context through real-world news and an adaptive, AI-powered spaced repetition tutor.**

[![Frontend](https://img.shields.io/badge/Frontend-React%2019%20%7C%20Vite%20%7C%20MUI-61dafb?style=flat-square&logo=react)](https://github.com/gminh715/vocab-mate-fe)
[![Backend](https://img.shields.io/badge/Backend-Python%203.12%20%7C%20FastAPI%20%7C%20SQLAlchemy%202.0-3776ab?style=flat-square&logo=fastapi)](https://github.com/gminh715/vocab-mate-be)
[![Database](https://img.shields.io/badge/Database-PostgreSQL%20%7C%20Asyncpg%20%7C%20Alembic-336791?style=flat-square&logo=postgresql)](https://www.postgresql.org/)
[![Algorithm](https://img.shields.io/badge/Algorithm-Py--FSRS--6.3%20Spaced%20Repetition-4caf50?style=flat-square)](https://github.com/open-spaced-repetition/py-fsrs)
[![AI Providers](https://img.shields.io/badge/AI%20Providers-Gemini%203.5%20Flash--Lite%20%7C%20Groq%20GPT--OSS--20B-ff6f00?style=flat-square&logo=google)](https://ai.google.dev/)
[![Docker](https://img.shields.io/badge/Docker-Compose%20Ready-2496ed?style=flat-square&logo=docker)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

🔗 **GitHub Repositories**:
- 🌐 **Frontend Client**: [https://github.com/gminh715/vocab-mate-fe](https://github.com/gminh715/vocab-mate-fe)
- ⚙️ **Backend API & AI Engine**: [https://github.com/gminh715/vocab-mate-be](https://github.com/gminh715/vocab-mate-be)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Project Architecture & Repository Structure](#-project-architecture--repository-structure)
- [Key Features](#-key-features)
- [News Ingestion Pipeline](#-news-ingestion-pipeline)
  - [Ingestion Flowchart](#ingestion-flowchart)
  - [Ingestion Stages Explained](#ingestion-stages-explained)
- [Tutor AI Agent Loop](#-tutor-ai-agent-loop)
  - [Tutor Agent Flowchart](#tutor-agent-flowchart)
  - [Tutor Agent Workflow & Architecture Principles](#tutor-agent-workflow--architecture-principles)
- [Technology Stack](#-technology-stack)
  - [Frontend Application](#frontend-application)
  - [Backend API & AI Orchestration](#backend-api--ai-orchestration)
- [Data Models & Schema Architecture](#-data-models--schema-architecture)
- [Trust Boundaries & Security Guardrails](#-trust-boundaries--security-guardrails)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [1. Backend Setup (Python + FastAPI)](#1-backend-setup-python--fastapi)
  - [2. Frontend Setup (React + Vite)](#2-frontend-setup-react--vite)
  - [3. One-Command Full-Stack (Docker Compose)](#3-one-command-full-stack-docker-compose)
  - [Environment Configurations](#environment-configurations)
- [Testing & Quality Assurance](#-testing--quality-assurance)
- [License](#-license)

---

## 🌟 Overview

**Vocab Mate** is a modern, context-first English language learning platform. Traditional flashcard systems isolate words from the natural syntactic structures and journalistic nuances in which native speakers actually use them. Vocab Mate bridges this gap by transforming authentic, high-caliber news journalism (such as *The Guardian*) into personalized, level-appropriate vocabulary learning.

Learners read authentic articles, tap unfamiliar words directly within sentences, view contextual definitions and Vietnamese translations, and retain their vocabulary through an **adaptive AI Tutor agent** driven by the **Free Spaced Repetition Scheduler (Py-FSRS 6.3)** algorithm.

---

## 🏛️ Project Architecture & Repository Structure

The platform is architected as a high-performance full-stack system consisting of a React SPA frontend and an asynchronous Python FastAPI backend:

```text
vocab-mate/
├── vocab-mate-frontend-python/     # React 19 + Vite + Material UI single-page application
├── vocab-mate-backend-python/      # Python 3.12 + FastAPI + SQLAlchemy 2.0 Async REST API & AI orchestrator
└── README.md                       # Root platform documentation
```

### Repository Breakdown

| Repository | GitHub Link | Tech Stack | Responsibilities |
| :--- | :--- | :--- | :--- |
| **Frontend** (`vocab-mate-fe`) | [`gminh715/vocab-mate-fe`](https://github.com/gminh715/vocab-mate-fe) | React 19.2, Vite 8.1, TypeScript 6.0, Material UI 9.2, TanStack Query 5, Tiptap 3 | Distraction-free article reader with CEFR word highlights, contextual term drawer, interactive tutor session studio, study streak calendars, FSRS memory distribution analytics, client-state caching, and Nginx production hosting. |
| **Backend** (`vocab-mate-be`) | [`gminh715/vocab-mate-be`](https://github.com/gminh715/vocab-mate-be) | Python 3.12+, FastAPI 0.141+, SQLAlchemy 2.0 (Async), PostgreSQL (asyncpg), Alembic, NLTK, Py-FSRS 6.3 | Asynchronous REST API (`/api/v1`), JWT authentication (in-memory access + HttpOnly cookie rotation), Guardian news ingestion gateway, DOM sentence/term annotation, NLTK local NLP & CEFR evaluation, Py-FSRS 6.3 scheduling, and dual-provider AI LLM orchestration (Gemini & Groq failover). |

---

## ✨ Key Features

- 📰 **Authentic News Reader**: Distraction-free reading experience with backend-sanitized HTML and stable DOM markers (`data-sentence-id`, `data-term-id`).
- 🎯 **Dynamic CEFR Highlighting**: Highlights vocabulary tailored to the user's CEFR proficiency (A1 to C2). Higher-difficulty words are visually highlighted to guide targeted acquisition.
- 🔍 **Contextual Word Lookup**: Click any marked word to reveal its IPA pronunciation, Vietnamese meaning specific to the sentence, contextual English definition, part of speech, and authentic usage examples.
- 💾 **Context-Bound Vocabulary Vault**: Words are permanently linked to the exact sentence and article where discovered, organized into custom user study collections.
- 🤖 **Adaptive AI Tutor Agent**: Daily personalized review sessions based on the learner's time budget (5, 10, 15, or 20 minutes) and current memory retention state.
- 🧠 **4 Dynamic Question Formats**:
  - `NEW` words $\rightarrow$ **Multiple Choice** (4 distinct contextual options A–D).
  - `LEARNING` words $\rightarrow$ **Contextual Cloze** (fill-in-the-blank `___` within the original sentence).
  - `REVIEW` words $\rightarrow$ **Typed Recall** (prompted in Vietnamese, typed in English).
  - `RELEARNING` (lapsed words) $\rightarrow$ **Micro-Lesson Retest** (fascinating real-world mini-stories embedding the target word, followed by instant verification).
- 💡 **Upfront Warmup Trivia Facts**: When reviewing lapsed words, the agent generates captivating real-world trivia facts (astronomy, history, biology, archaeology) connecting to the target vocabulary to activate prior memory.
- 📈 **Py-FSRS Spaced Repetition**: Memory stability, difficulty, review intervals, and scheduled due dates are mathematically scheduled using `fsrs` (v6.3).
- 📊 **Learner Analytics & Streaks**: Real-time tracking of retention rate, FSRS memory distribution, study streaks, and CEFR progress.
- 🛠️ **Admin Ingestion & Publishing Studio**: Search The Guardian API, import articles as drafts, run automated sentence parsing and CEFR difficulty analysis, and publish live content.

---

## 📰 News Ingestion Pipeline

The News Ingestion Pipeline discovers, deduplicates, sanitizes, tokenizes, and linguistically analyzes authentic journalism before making it available to learners.

### Ingestion Flowchart

```mermaid
flowchart TD
    subgraph Discovery["1. Discovery & Fetching"]
        A["Ingestion Trigger<br/>Admin UI or Scheduled API Sync"] --> B["News Source Gateway<br/>The Guardian Open Platform API"]
        B --> C["Fetch Metadata & Raw Article Payloads<br/>Batch Limit: 1-10 articles"]
    end

    subgraph Normalization["2. Normalization & Sanitization"]
        C --> D["Duplicate Check via External ID<br/>articles.external_id"]
        D -- "Duplicate Exists" --> D_SKIP["Skip Import & Log"]
        D -- "Unique Article" --> E["HTML Sanitization & Extraction<br/>BeautifulSoup4 + lxml tag allowlist"]
        E --> F["Generate Canonical Slug & Invariant Hash<br/>SHA-256 Suffix"]
        F --> G["Resolve / Create Category<br/>CategoriesService"]
        G --> H["Create Article Draft in PostgreSQL<br/>Status: DRAFT, content_version: 1"]
    end

    subgraph Parsing["3. Sentence Parsing & Annotation"]
        H --> I["Parse Reading Sentences<br/>NLTK sent_tokenize"]
        I --> J["Annotate DOM with Sentence IDs<br/>data-sentence-id"]
        J --> K["Persist ArticleSentence Records<br/>Async SQLAlchemy Session"]
    end

    subgraph Analysis["4. Local NLP & CEFR Complexity Analysis"]
        K --> L["Claim Concurrency Lock on Draft<br/>claim_article_analysis"]
        L --> M["NLTK Tokenization & Lemmatization<br/>word_tokenize + WordNetLemmatizer + pos_tag"]
        M --> N["Classify CEFR Vocabulary Levels: A1-C2<br/>Local Oxford CEFR Lexicon Mapping"]
        N --> O["Compute Overall Article CEFR Complexity"]
        O --> P["Extract Candidate Terms & Tag DOM<br/>Inject data-term-id via TermMarkerHelper"]
        P --> Q["Persist ArticleSentenceTerm Records"]
    end

    subgraph Publication["5. AI Enrichment & Publication Gate"]
        Q --> R{"Publication Gate Validation<br/>Sentences > 0, Terms tagged, CEFR evaluated"}
        R -- "Checks Failed" --> R_FAIL["Retain Draft + Store Diagnostics"]
        R -- "Checks Passed" --> S["Atomic Publication<br/>Status: PUBLISHED"]
        S --> T["On-Demand / Lazy AI Term Enrichment<br/>Triggered on reader click or pre-cache"]
        T --> U["Primary LLM: Google Gemini 3.5 Flash-Lite<br/>SDK: google-genai (Minimal Thinking)"]
        U -- "Quota / Rate Limit / Timeout" --> V["Fallback LLM: Groq openai/gpt-oss-20b<br/>SDK: groq async (Strict JSON Schema)"]
        U -- "Success" --> W["Persist Contextual Translation, IPA & Examples<br/>Vietnamese meaning, POS, CEFR"]
        V --> W
    end
```

### Ingestion Stages Explained

1. **Source Discovery**:
   - The backend queries **The Guardian Open Platform API** by section, keywords, date ranges, and publication order (`newest`, `oldest`, `relevance`).
2. **Deduplication & Canonicalization**:
   - Each article is verified against `articles.external_id`.
   - Slugs are generated from normalized title strings appended with a 12-character SHA-256 digest of the source URL (`import_slug`) to guarantee invariant URLs.
3. **HTML Sanitization**:
   - Raw HTML is scrubbed of dangerous tags, embedded scripts, and trackers using strict tag allowlists via **BeautifulSoup4** and **lxml**.
4. **Sentence Segmentation**:
   - **NLTK (`sent_tokenize`)** segments sanitized text into grammatical reading sentences, wrapping each in `<span data-sentence-id="uuid">`.
5. **Local NLP & CEFR Analysis**:
   - **NLTK (`word_tokenize`, `WordNetLemmatizer`, `pos_tag`)** extracts lemmas, parts of speech, and filters punctuation and stopwords.
   - Matches tokens against cached Oxford CEFR lexicons (A1, A2, B1, B2, C1, C2) and calculates overall article readability complexity.
   - Identified vocabulary terms are stamped into the markup with `<span data-term-id="uuid">` and saved as `ArticleSentenceTerm` rows.
6. **Lazy AI Enrichment**:
   - When a user inspects a term in the reader, FastAPI calls `AiService.enrich_contextual_term()`.
   - **Google Gemini 3.5 Flash-Lite** (with **Groq openai/gpt-oss-20b** as an automatic failover) enriches the term with IPA pronunciation, Vietnamese meaning specific to the sentence, contextual English definition, and two usage examples.

---

## 🤖 Tutor AI Agent Loop

The Tutor AI Agent orchestrates a daily adaptive vocabulary study loop. It determines which vocabulary items need review, crafts dynamic contextual learning tasks, and updates the learner's spaced repetition memory matrix.

### Tutor Agent Flowchart

```mermaid
flowchart TD
    subgraph SessionInit["1. Session Initialization & Daily Budgeting"]
        A["Learner Starts / Resumes Session<br/>POST /api/v1/tutor/session"] --> B["Resolve Study Date<br/>Timezone: Asia/Ho_Chi_Minh"]
        B --> C{"Active Session Exists Today?"}
        C -- "Yes - ACTIVE" --> D["Restore Persisted Session & Active Item"]
        C -- "No - Fresh Day" --> E["Compute Session Budget<br/>daily_study_minutes to target_activity_count"]
        E --> F["Allocate New Word Target<br/>~20% of activities, min 1"]
        F --> G{"Any RELEARNING Words Due?"}
        G -- "Yes" --> H["AI Generates Warmup Trivia Facts<br/>Real-world facts embedding target words"]
        G -- "No" --> I["Create TutorSession Record<br/>Status: ACTIVE"]
        H --> I
    end

    subgraph CandidateSelection["2. Py-FSRS Priority Queue & Item Dispatch"]
        I --> J["Fetch Prioritized Candidate Pool<br/>Bounded Limit: 50 items"]
        D --> J
        J --> K["Priority Ordering:<br/>1. RELEARNING: due, next_review_at <= now<br/>2. LEARNING: due, next_review_at <= now<br/>3. REVIEW: due or overdue, oldest first<br/>4. NEW: oldest saved_at first"]
        K --> L["Select Target Candidate<br/>Deduplicate words already tested in this session"]
        L --> M{"Map FSRS State to Question Type"}
        M -- "NEW" --> N1["MULTIPLE_CHOICE<br/>4 distinct options A-D"]
        M -- "LEARNING" --> N2["CONTEXTUAL_CLOZE<br/>Sentence with blank ___"]
        M -- "REVIEW" --> N3["TYPED_RECALL<br/>Vietnamese prompt to Type English word"]
        M -- "RELEARNING" --> N4["MICRO_LESSON_RETEST<br/>Mini-story trivia + instant retest"]
    end

    subgraph AIGeneration["3. Bounded AI Question Generation"]
        N1 & N2 & N3 & N4 --> O["Structured AI Generation Request<br/>Pydantic v2 & Strict JSON Schemas"]
        O --> P["Primary Provider: Google Gemini 3.5 Flash-Lite"]
        P -- "Quota / Timeout / Error" --> Q["Fallback Provider: Groq openai/gpt-oss-20b"]
        P -- "Success" --> R["Parse & Segregate Payloads"]
        Q --> R
        R --> S1["Public question_payload<br/>Prompt, options, cloze - No correct answer"]
        R --> S2["Private grading_spec<br/>Stored server-side: answer, explanation, feedback"]
        S1 & S2 --> T["Persist TutorSessionItem<br/>Status: PENDING"]
    end

    subgraph Interaction["4. Learner Interaction & Answer Submission"]
        T --> U["Learner Solves Question<br/>Client records response_time_ms, hint_used"]
        U --> V["Submit Answer<br/>POST /items/:itemId/answer"]
    end

    subgraph Evaluation["5. Deterministic Grading & Py-FSRS Scheduling"]
        V --> W["Deterministic Server-Side Grading<br/>normalize_typed_answer: trim + lowercase"]
        W --> X["Compute FSRS Rating Grade:<br/>- Incorrect -> Again: Grade 1<br/>- Hint used OR response_time >= 30s -> Hard: Grade 2<br/>- MULTIPLE_CHOICE correct -> Hard ceiling: Grade 2<br/>- Fast TYPED_RECALL under 5s & reps >= 3 -> Easy: Grade 4<br/>- Standard Correct -> Good: Grade 3"]
        X --> Y["Run Py-FSRS 6.3 Algorithm:<br/>- Recompute stability & difficulty<br/>- Calculate next interval: 1-60+ days<br/>- Assign next_review_at UTC timestamp"]
        Y --> Z["Atomic SQLAlchemy Transaction:<br/>- Update UserVocabulary FSRS fields<br/>- Mark TutorSessionItem as ANSWERED<br/>- Store feedback_vi & accuracy"]
    end

    subgraph LoopCheck["6. Loop Condition & Completion"]
        Z --> AA{"Session Target Reached?<br/>answered_count >= target_activity_count"}
        AA -- "No - Activities Remain" --> L
        AA -- "Yes - Session Completed" --> AB["Mark Session COMPLETED<br/>completed_at = now"]
        AB --> AC["Generate Summary Statistics<br/>Accuracy %, Words Learned, Retention, Streak"]
    end
```

### Tutor Agent Workflow & Architecture Principles

1. **Deterministic Grading Guardrail**:
   - The LLM **never** grades user answers. LLMs are susceptible to hallucinations, tone bias, and non-deterministic evaluation.
   - Grading is executed 100% deterministically on the FastAPI backend via `normalize_typed_answer()` (strict whitespace trimming and lowercase comparison).
2. **Asymmetric Data Protection**:
   - `question_payload` (sent to client) contains only the prompt, options, or sentence blank.
   - `grading_spec` (correct answer, pedagogical explanations, teacher feedback) is stored server-side and is **never** serialized to the frontend while the item is `PENDING`.
3. **Py-FSRS 6.3 Multi-Factor Rating Policy**:
   - **`Again` (1)**: Incorrect response. Reschedules item for rapid relearning.
   - **`Hard` (2)**: Correct response, but required a hint, took $\ge 30\text{s}$, or was a Multiple Choice recognition question.
   - **`Good` (3)**: Accurate recall under normal pacing without hints.
   - **`Easy` (4)**: Instantaneous typed recall ($< 5\text{s}$) on an established word reviewed at least 3 times.
4. **Fascinating Micro-Lessons for Lapsed Words**:
   - When reviewing a forgotten word (`RELEARNING`), the agent generates an engaging real-world mini-story or trivia fact (archaeology, biology, astronomy, history) embedding the target word in bold, followed by an immediate contextual re-test.
5. **Session Invariant Guarantee**:
   - Exactly one session per user per day based on `Asia/Ho_Chi_Minh` timezone. Active sessions are automatically resumed without creating duplicate records.

---

## 🧰 Technology Stack

### Frontend Application

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Framework** | [React 19.2](https://react.dev/) + [Vite 8.1](https://vitejs.dev/) | High-performance reactive UI library with instant HMR |
| **Language** | [TypeScript 6.0](https://www.typescriptlang.org/) | Strict static typing across components, hooks, and API contracts |
| **Design System**| [Material UI 9.2 (MUI)](https://mui.com/) + Emotion | Accessible component system with custom theme tokens and dark mode |
| **Rich Text Editor**| [Tiptap 3.28](https://tiptap.dev/) | Extensible rich-text editor for administrative article authoring |
| **Data Fetching** | [TanStack Query 5.101](https://tanstack.com/query) | Server-state caching, automatic invalidation, and deduplication |
| **Routing** | [React Router 7.18](https://reactrouter.com/) | Client-side routing with role-based route guards and query param management |
| **Forms & Validation**| [React Hook Form 7.82](https://react-hook-form.com/) + [Zod 4.4](https://zod.dev/) | High-performance schema-validated form workflows |
| **HTTP Client** | [Axios 1.18](https://axios-http.com/) | Configured client with automatic silent JWT refresh retry interceptor |
| **Internationalization**| [i18next 26.3](https://www.i18next.com/) + `react-i18next` | Multilingual support (Vietnamese & English) |
| **Production Server**| [Nginx 1.27 Alpine](https://nginx.org/) | Lightweight production web server with SPA routing fallback & reverse proxy |
| **Testing** | [Vitest 4.1](https://vitest.dev/) + Testing Library | Unit and component integration tests |

### Backend API & AI Orchestration

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Framework** | [FastAPI 0.141+](https://fastapi.tiangolo.com/) | High-performance asynchronous REST API framework (ASGI) |
| **Runtime & Manager**| [Python 3.12+](https://www.python.org/) + [uv](https://github.com/astral-sh/uv) | Modern, fast Python dependency and project management |
| **Server Engine** | [Uvicorn](https://www.uvicorn.org/) | Lightning-fast ASGI production server |
| **Database & ORM** | [PostgreSQL](https://www.postgresql.org/) + [SQLAlchemy 2.0 Async](https://www.sqlalchemy.org/) | AsyncIO ORM queries with `asyncpg` driver |
| **Migrations** | [Alembic 1.20+](https://alembic.sqlalchemy.org/) | Database schema revision management and automatic migrations |
| **NLP Engine** | [NLTK 3.10+](https://www.nltk.org/) + [BeautifulSoup4](https://www.crummy.com/software/BeautifulSoup/) + [lxml](https://lxml.de/) | Text segmentation, POS tagging, WordNet lemmatization, and HTML sanitization |
| **CEFR Scoring** | Built-in Lexicon Matcher | Oxford CEFR dictionary mapping (A1–C2) and weighted complexity calculation |
| **Spaced Repetition**| [Py-FSRS 6.3](https://github.com/open-spaced-repetition/py-fsrs) | Authoritative Free Spaced Repetition Scheduler algorithm |
| **Primary AI Model** | [Google Gemini 3.5 Flash-Lite](https://ai.google.dev/) | Primary structured LLM via `google-genai` with minimal thinking config |
| **Fallback AI Model**| [Groq openai/gpt-oss-20b](https://groq.com/) | Low-latency failover LLM via `groq` with strict JSON schema mode |
| **Media Storage** | [Cloudinary](https://cloudinary.com/) | Cloud storage for user profile avatar uploads |
| **Auth & Security** | [PyJWT](https://pyjwt.readthedocs.io/) + [pwdlib (bcrypt)](https://github.com/frankie567/pwdlib) | Short-lived access JWTs + rotated HttpOnly refresh cookies + secure hashing |
| **API Documentation**| [Swagger UI / OpenAPI](https://fastapi.tiangolo.com/features/) | Auto-generated interactive API documentation at `/docs` and `/redoc` |
| **Testing & Linting**| [pytest 9.1+](https://pytest.org/) + [ruff 0.16+](https://github.com/astral-sh/ruff) | Async unit/integration tests, strict linting, and formatting |

---

## 🗄️ Data Models & Schema Architecture

Vocab Mate uses PostgreSQL with asynchronous SQLAlchemy 2.0 declarative models, modularly organized in `app/models/`:

```text
app/models/
├── articles.py        # Article, ArticleSentence, ArticleSentenceTerm
├── tutor.py           # TutorSession, TutorSessionItem (payload & grading spec)
├── vocabularies.py    # UserVocabulary (FSRS memory stability, difficulty, due dates)
├── collections.py     # UserCollection, CollectionItem
├── reading.py         # UserReadingHistory, scroll progress, completion flags
├── categories.py      # Article categories (e.g., Science, Technology, Culture)
├── users.py           # User profiles, CEFR levels, daily study targets, credentials
└── enums.py           # CefrLevel, FsrsCardState, TutorQuestionType, ArticleStatus
```

### Key Relational Entities

- **`Article`**: Stores `content_html`, `content_version`, `cefr_level`, and publication state (`DRAFT`, `PUBLISHED`, `ARCHIVED`).
- **`ArticleSentence`**: Child of `Article`. Segmented sentence annotated with `data-sentence-id`.
- **`ArticleSentenceTerm`**: Child of `ArticleSentence`. Identified vocabulary tagged with `data-term-id`, lemma, part of speech, and CEFR level.
- **`UserVocabulary`**: Links a user to an `ArticleSentenceTerm`. Stores a contextual snapshot of the word in its original sentence alongside full FSRS memory parameters:
  - `fsrs_state`: `NEW`, `LEARNING`, `REVIEW`, or `RELEARNING`.
  - `fsrs_stability`, `fsrs_difficulty`, `fsrs_scheduled_days`, `review_count`, `lapse_count`.
  - `next_review_at`: Exact UTC timestamp for the next review due date.
- **`TutorSession`**: Daily study session record unique by `[user_id, study_date]`.
- **`TutorSessionItem`**: Individual activity within a session holding:
  - `question_payload`: Public JSON sent to the frontend client.
  - `grading_spec`: Server-only JSON containing correct answers and pedagogical explanations.
  - `fsrs_rating`: Numerical rating (1–4) assigned after deterministic evaluation.

---

## 🔒 Trust Boundaries & Security Guardrails

1. **Authentication & Session Tokens**:
   - Access tokens are stored **in-memory only** on the frontend client (never in `localStorage` or `sessionStorage`).
   - Refresh tokens are transmitted via an **`HttpOnly`, `SameSite=Lax`, `Secure`** cookie.
   - Axios request interceptors automatically refresh expired tokens concurrently, queuing in-flight requests and retrying failed calls exactly once.
2. **Zero-Trust AI Guardrails**:
   - LLMs are treated as untrusted data generators.
   - All AI responses are validated through strict **Pydantic v2 schemas** before database persistence.
   - Prompts include explicit boundary instructions preventing prompt injection from article texts.
   - AI outputs never determine authorization, scores, user roles, or grading correctness.
3. **Deterministic Grading Separation**:
   - Learner answers are verified exclusively by server-side deterministic logic (`normalize_typed_answer()`), never by LLMs.
4. **Bounded External Requests**:
   - The Guardian API queries enforce bounded pagination sizes, timeout protections, and safe error masking.

---

## 🚀 Getting Started

### Prerequisites

Ensure the following tools are installed on your system:

- **Python**: `>=3.12` and [**`uv`**](https://github.com/astral-sh/uv)
- **Node.js**: `>=22.0.0` (LTS) and **npm**: `>=10.0.0`
- **PostgreSQL**: `v15+` (local instance or Supabase)
- **Docker & Docker Compose** *(optional, for one-command containerized run)*
- **API Keys**:
  - [The Guardian Open Platform API Key](https://open-platform.theguardian.com/access/)
  - [Google AI Studio (Gemini) API Key](https://aistudio.google.com/)
  - [Groq Cloud API Key](https://console.groq.com/)
  - *(Optional)* [Cloudinary Account](https://cloudinary.com/) (for user avatar uploads)

---

### 1. Backend Setup (Python + FastAPI)

> **Repository**: [https://github.com/gminh715/vocab-mate-be](https://github.com/gminh715/vocab-mate-be)

```bash
# Navigate to backend directory
cd vocab-mate-backend-python

# Configure environment variables
cp .env.example .env

# Install dependencies and create virtual environment using uv
uv sync

# Download required NLTK tokenizers and language models
uv run python -m nltk.downloader punkt punkt_tab averaged_perceptron_tagger averaged_perceptron_tagger_eng wordnet

# Apply database migrations
uv run alembic upgrade head

# Start FastAPI development server with hot-reload
uv run uvicorn app.main:app --reload --port 3000
```

The FastAPI backend will listen on `http://localhost:3000`.  
- **Interactive Swagger Documentation**: `http://localhost:3000/docs`  
- **Alternative ReDoc UI**: `http://localhost:3000/redoc`  
- **Health Check Probe**: `http://localhost:3000/health/live`  

---

### 2. Frontend Setup (React + Vite)

> **Repository**: [https://github.com/gminh715/vocab-mate-fe](https://github.com/gminh715/vocab-mate-fe)

```bash
# Open a new terminal and navigate to frontend directory
cd vocab-mate-frontend-python

# Install dependencies
npm ci

# Configure environment variables
cp .env.example .env

# Start frontend development server
npm run dev
```

The React frontend will be accessible at `http://localhost:5173`.

---

### 3. One-Command Full-Stack (Docker Compose)

Run both Backend (FastAPI) and Frontend (Nginx SPA) in isolated containers:

```bash
cd vocab-mate-backend-python

# Build and start all services in detached mode
docker compose up --build -d

# View real-time container logs
docker compose logs -f
```

- **Frontend Client**: `http://localhost:5173` (or `http://localhost:80`)
- **Backend API**: `http://localhost:3000`

---

### Environment Configurations

#### Backend (`vocab-mate-backend-python/.env`)

```ini
# Server configuration
PORT=3000
HOST=0.0.0.0
ENVIRONMENT=development

# Database configuration (PostgreSQL via asyncpg)
DATABASE_URL=postgresql+asyncpg://postgres:postgres@localhost:5432/vocab_mate
DIRECT_URL=postgresql://postgres:postgres@localhost:5432/vocab_mate

# Authentication & JWT
JWT_ACCESS_SECRET=your-high-entropy-access-secret-minimum-32-characters
JWT_ACCESS_EXPIRES_IN=900
JWT_REFRESH_SECRET=your-high-entropy-refresh-secret-minimum-32-characters
JWT_REFRESH_EXPIRES_IN=604800
BCRYPT_ROUNDS=12

# CORS & Cookies
CORS_ORIGIN=http://localhost:5173,http://localhost:3000
COOKIE_SECURE=false
COOKIE_SAME_SITE=lax

# Timezone
ANALYTICS_TIMEZONE=Asia/Ho_Chi_Minh

# AI Providers
GEMINI_API_KEY=AIzaSy...
GEMINI_MODEL=gemini-3.5-flash-lite
GROQ_API_KEY=gsk_...
GROQ_MODEL=openai/gpt-oss-20b
AI_REQUEST_TIMEOUT_MS=30000

# Guardian Content API
GUARDIAN_API_KEY=your-guardian-api-key

# Cloudinary (User Avatars)
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret
CLOUDINARY_FOLDER=vocab-mate/avatars
```

#### Frontend (`vocab-mate-frontend-python/.env`)

```ini
VITE_API_BASE_URL=/api/v1
VITE_API_PROXY_TARGET=http://localhost:3000
```

---

## 🧪 Testing & Quality Assurance

### Backend Quality Checks

```bash
cd vocab-mate-backend-python

# Run test suite with pytest
uv run pytest

# Check code formatting & linting with ruff
uv run ruff check .

# Auto-format codebase
uv run ruff format .
```

### Frontend Quality Checks

```bash
cd vocab-mate-frontend-python

# TypeScript static type check
npm run typecheck

# ESLint inspection
npm run lint

# Component and unit tests with Vitest
npm test

# Production bundle build check
npm run build
```

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

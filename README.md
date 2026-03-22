<div align="center">

# ⚡ Edynapse — AI Educational Content Engine


**Curriculum-aligned educational content — generated, reviewed, and refined autonomously.**



---

</div>

## 📌 Overview

**Edynapse** is a B2B EdTech platform that uses an **agentic AI pipeline** to generate structured educational content for Grades 1–12. Built on **LangGraph** and powered by **Google Gemini 2.5 Flash**, it autonomously generates explanations and MCQs, reviews them against a quality rubric, and self-corrects on failure — all without human intervention.

---

## 🏗️ ArchitectureSTART
│
▼
┌─────────────┐     structured      ┌──────────────┐
│  Generator  │ ──── Content ──────▶│   Reviewer   │
│   Node      │                     │    Node      │
└─────────────┘                     └──────┬───────┘
▲                                   │
│            fail + retries left    │ pass
│◀──────────────────────────────────┤
│                                   │
(retry once)                             ▼
END

| Node | Role | Model |
|---|---|---|
| **Generator** | Produces `explanation` + 5 MCQs as structured JSON | Gemini 2.5 Flash |
| **Reviewer** | Evaluates against a 3-criterion rubric, returns `pass`/`fail` | Gemini 2.5 Flash |
| **Router** | Sends back to Generator once on `fail`; ends on `pass` or retry exhaustion | — |

---

## ✨ Features

- 🧠 **Agentic self-correction** — automatically retries once on review failure
- 📐 **Structured output** — uses `with_structured_output()` + Pydantic validation
- 🎓 **Grade-aware generation** — calibrated language for Grades 1–12
- 📝 **5 MCQs per lesson** — each testing a different concept from the explanation
- 🔍 **Rubric-based review** — grade appropriateness, explanation quality, MCQ quality
- 📥 **Markdown export** — download a beautifully structured `.md` file
- 🖥️ **Professional B2B UI** — dark theme Streamlit dashboard with KPI stats

---

## 🗂️ Project Structureedynapse-gemini/
│
├── app.py                  # Streamlit frontend (B2B dashboard UI)
├── Agent_orchestration.py  # LangGraph pipeline (generator + reviewer + router)
├── requirements.txt        # Python dependencies
├── .env                    # Local secrets (never commit this)
├── .env.example            # Template for environment variables
└── README.md

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- A Google AI Studio API key → [Get one here](https://aistudio.google.com/apikey)

### 1. Clone the repository
```
git clone https://github.com/swatibuilds/Edynapse-AI-Content-Engine.git
cd Edynapse-AI-Content-Engine

### 2. Install dependencies
```bashpip install -r requirements.txt

### 3. Set up environment variables
```bashcp .env.example .env

Edit `.env`:
```envGOOGLE_API_KEY="your-google-api-key-here"

### 4. Run locally
```bashstreamlit run app.py

---
```
## ☁️ Deploying to Streamlit Cloud

1. Push your repo to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io) → **New app**
3. Select your repo and set `app.py` as the main file
4. Open **Advanced settings → Secrets** and add:
```tomlGOOGLE_API_KEY = "your-google-api-key-here"
```
5. Click **Deploy** ✅

> ⚠️ Never commit your `.env` file or API key to GitHub.

---

## 📦 Dependenciesstreamlit
langgraph
langchain
langchain-google-genai
google-generativeai
python-dotenv
pydantic
langchain-core

---

## 🔑 Environment Variables

| Variable | Description | Required |
|---|---|---|
| `GOOGLE_API_KEY` | Google AI Studio API key for Gemini access | ✅ Yes |

---

## 🧠 How the Pipeline Works

### Generator Node
Receives `grade` and `topic`, sends a structured prompt to **Gemini 2.5 Flash** via `with_structured_output(Content)`. Returns a validated `Content` object containing:
- A three-part explanation (INTRO → CONCEPTS → SUMMARY)
- Exactly 5 MCQs, each testing a different concept

### Reviewer Node
Receives the `Content` object, evaluates it against a 3-criterion rubric:
1. **Grade Appropriateness** — language and vocabulary match the grade
2. **Explanation Quality** — structure, accuracy, and completeness
3. **MCQ Quality** — format, coverage, and plausibility of distractors

Returns a `Review` object with `status: "pass" | "fail"` and actionable `feedback`.

### Router
- `pass` → pipeline ends, output surfaced to UI
- `fail` + retries remaining → sends feedback back to Generator
- `fail` + retries exhausted → best-effort output surfaced to UI

---

## 📸 UI Overview

| Section | Description |
|---|---|
| **Hero** | Edynapse branding, product tagline |
| **Left Panel** | Grade selector, topic input, pipeline diagram |
| **Stat Row** | Generation time, retries used, MCQ count, review status |
| **Output Tab** | Explanation card + MCQ cards with highlighted correct answers |
| **Inspector Tab** | Raw JSON from generator + reviewer, execution logs |
| **Export** | One-click Markdown download |

---

## 📄 License

MIT License © 2024 Edynapse

---

<div align="center">
Built with ⚡ by the Edynapse team
</div>

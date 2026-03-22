⚡ Edynapse — AI Educational Content Engine

Curriculum-aligned educational content — generated, reviewed, and refined autonomously.




📌 Overview

Edynapse is a B2B EdTech platform that uses an agentic AI pipeline to generate structured educational content for Grades 1–12. Built on LangGraph and powered by Google Gemini 2.5 Flash, it autonomously generates explanations and MCQs, reviews them against a quality rubric, and self-corrects on failure — all without human intervention.

🏗️ Architecture
START
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
NodeRoleModelGeneratorProduces explanation + 5 MCQs as structured JSONGemini 2.5 FlashReviewerEvaluates against a 3-criterion rubric, returns pass/failGemini 2.5 FlashRouterSends back to Generator once on fail; ends on pass or retry exhaustion—

✨ Features

🧠 Agentic self-correction — automatically retries once on review failure
📐 Structured output — uses with_structured_output() + Pydantic validation
🎓 Grade-aware generation — calibrated language for Grades 1–12
📝 5 MCQs per lesson — each testing a different concept from the explanation
🔍 Rubric-based review — grade appropriateness, explanation quality, MCQ quality
📥 Markdown export — download a beautifully structured .md file
🖥️ Professional B2B UI — dark theme Streamlit dashboard with KPI stats


🗂️ Project Structure
edynapse-gemini/
│
├── app.py                  # Streamlit frontend (B2B dashboard UI)
├── Agent_orchestration.py  # LangGraph pipeline (generator + reviewer + router)
├── requirements.txt        # Python dependencies
├── .env                    # Local secrets (never commit this)
├── .env.example            # Template for environment variables
└── README.md

🚀 Getting Started
Prerequisites

Python 3.10+
A Google AI Studio API key → Get one here

1. Clone the repository
bashgit clone https://github.com/yourusername/edynapse-gemini.git
cd edynapse-gemini
2. Install dependencies
bashpip install -r requirements.txt
3. Set up environment variables
bashcp .env.example .env
Edit .env:
envGOOGLE_API_KEY="your-google-api-key-here"
4. Run locally
bashstreamlit run app.py

☁️ Deploying to Streamlit Cloud

Push your repo to GitHub
Go to share.streamlit.io → New app
Select your repo and set app.py as the main file
Open Advanced settings → Secrets and add:

tomlGOOGLE_API_KEY = "your-google-api-key-here"

Click Deploy ✅


⚠️ Never commit your .env file or API key to GitHub.


📦 Dependencies
streamlit
langgraph
langchain
langchain-google-genai
google-generativeai
python-dotenv
pydantic
langchain-core

🔑 Environment Variables
VariableDescriptionRequiredGOOGLE_API_KEYGoogle AI Studio API key for Gemini access✅ Yes

🧠 How the Pipeline Works
Generator Node
Receives grade and topic, sends a structured prompt to Gemini 2.5 Flash via with_structured_output(Content). Returns a validated Content object containing:

A three-part explanation (INTRO → CONCEPTS → SUMMARY)
Exactly 5 MCQs, each testing a different concept

Reviewer Node
Receives the Content object, evaluates it against a 3-criterion rubric:

Grade Appropriateness — language and vocabulary match the grade
Explanation Quality — structure, accuracy, and completeness
MCQ Quality — format, coverage, and plausibility of distractors

Returns a Review object with status: "pass" | "fail" and actionable feedback.
Router

pass → pipeline ends, output surfaced to UI
fail + retries remaining → sends feedback back to Generator
fail + retries exhausted → best-effort output surfaced to UI


📸 UI Overview
SectionDescriptionHeroEdynapse branding, product taglineLeft PanelGrade selector, topic input, pipeline diagramStat RowGeneration time, retries used, MCQ count, review statusOutput TabExplanation card + MCQ cards with highlighted correct answersInspector TabRaw JSON from generator + reviewer, execution logsExportOne-click Markdown download

📄 License
MIT License © 2024 Edynapse

<div align="center">
Built with ⚡ by the Edynapse team
</div>

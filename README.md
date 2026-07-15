# SkillBridge AI 🎯
### **Empowering Informal Workers through Generative AI Mentorship**
Built with **IBM watsonx.ai Granite** on **IBM Cloud**

---

## 🌟 Overview

**SkillBridge AI** is an award-winning, inclusive career mentorship ecosystem specifically engineered for the informal labor economy. It serves as an AI-powered assistant for daily wage laborers, gig workers, artisans, street vendors, domestic workers, delivery partners, farmers, construction laborers, and unemployed youth. 

By leveraging the enterprise-ready power of **IBM watsonx.ai Granite models**, SkillBridge AI democratizes career growth, helps optimize income potential, identifies skill deficiencies, and bridges the information asymmetry surrounding government welfare programs.

---

## ✨ System Blueprint

The architectural flow illustrates how user inputs move dynamically from frontend modules through the secure Flask orchestrator, updating session states and triggering discrete task vectors handled by the IBM watsonx.ai engine.

---

## 🚀 Key Features

### 🧠 Core AI Capabilities
* 🤖 **Context-Aware AI Mentor:** Hyper-personalized career advice driven by the `ibm/granite-13b-instruct-v2` foundational model.
* 📊 **Dynamically Updated User Dashboard:** Real-time analytics tracking profile completeness, calculated resume scores, interview preparation readiness, and potential match tracking.
* 🎯 **Visual Career Roadmaps:** Step-by-step career milestones mapped dynamically against the user's localized target goals.
* 🔍 **Skill Gap & Curated Course Engine:** Algorithmic translation comparing a worker's current profile metrics against market requirements to flag actionable learning resources.

### 💼 Career Tools
* 💼 **Smart Job Profiling:** Hyper-localized recommendation matches tailored around explicit skill levels and logistical conditions.
* 🏛 **Welfare Scheme Discovery:** Personalized lookup engine highlighting matching Central & State government programs (e.g., PM schemes) with absolute safety limits against data hallucination.
* 📝 **ATS-Optimized Resume Analysis:** Direct string parsing engine grading resumes against career paths and outputting corrective feedback.
* 🎤 **Interactive Interview Coach:** Role-specific mock interview question sets complete with confidence tips and behavioral guidelines.

### 🌐 Inclusivity & Accessibility Add-ons
* 🎙 **Voice-to-Text Inputs:** Native microphone capture designed for low-literacy accessibility.
* 🌐 **Native Multilingual UI:** Seamless support across English, Hindi, and colloquial Hinglish phrasing.
* 🌙 **Adaptive Layouts:** Toggle-ready Dark Mode optimized for low-power mobile layouts.

---

## 🛠 Technology Stack

| Tier | Component Description |
| :--- | :--- |
| **Backend Core** | Python Flask 3.x (with isolated server-side `filesystem` state storage) |
| **AI Orchestration** | IBM watsonx.ai foundational LLM client API wrapper |
| **Model Node** | `ibm/granite-13b-instruct-v2` |
| **Frontend Layout** | Bootstrap 5, HTML5 Semantics, Modern Vanilla JavaScript (ES6+) |
| **Environment Control** | `python-dotenv` framework |
| **Production WSGI** | Gunicorn (Green Unicorn HTTP Server) |

---

## 📁 Project Workspace Anatomy

```text
SkillBridge-AI/
│
├── app.py                      # Flask Application Entry Point (Routing & API Endpoints)
├── requirements.txt            # Python Dependencies Manifest
├── README.md                   # Repository Documentation
├── .env.example                # Blueprint for secure environment setups
│
├── services/
│   └── granite_service.py      # Core SDK implementation wrapper for IBM watsonx.ai
│
├── templates/                  # Presentation Layers (JinJA2 Engine)
│   ├── index.html              # Dynamic App Landing Frame
│   ├── dashboard.html          # Performance Metrics Overview Control panel
│   ├── chatbot.html            # Conversational Interface Canvas
│   ├── jobs.html               # Targeted Placement Opportunities
│   ├── schemes.html            # Validated Government Schemes Hub
│   └── profile.html            # Profile Builder & Completeness Metric Module
│
└── static/                     # Global Static Materials Asset Pipeline
    ├── css/style.css           # Global Stylesheets & Layout Modifiers
    ├── js/script.js            # Client-side Core Logic & API Fetch Controllers
    └── images/                 # Theme graphics

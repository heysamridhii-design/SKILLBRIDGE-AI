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
---
---

## ⚡ Quick Start Deployment Guide

### 1. Clone & Access the Workspace
```bash
git clone [https://github.com/your-username/skillbridge-ai.git](https://github.com/your-username/skillbridge-ai.git)
cd skillbridge-ai
2. Isolate Dependencies (Virtual Environment)
Bash
# Initialize Environment
python -m venv venv

# Activate Environment (Linux/macOS)
source venv/bin/activate

# Activate Environment (Windows)
venv\Scripts\activate
3. Install Module Matrix
Bash
pip install --upgrade pip
pip install -r requirements.txt
4. Inject Local Environment Configuration
Duplicate the configuration template file into an active configuration state:

Bash
cp .env.example .env
Populate the newly created .env parameters with your active parameters:

Code snippet
IBM_CLOUD_API_KEY=your_secure_ibm_cloud_api_key
PROJECT_ID=your_watsonx_ai_project_guid
MODEL_ID=ibm/granite-13b-instruct-v2
IBM_WATSONX_URL=[https://us-south.ml.cloud.ibm.com](https://us-south.ml.cloud.ibm.com)
FLASK_SECRET_KEY=generate_a_secure_hex_key_here
FLASK_DEBUG=false
PORT=5000
5. Launch the Framework Locally
Bash
python app.py
Your service instance will instantly serve requests at: http://localhost:5000
🔑 Provisioning IBM watsonx.ai InfrastructureAccess the Cloud Console: Create an IBM Cloud Instance at cloud.ibm.com.Launch the Workspace: Spin up a new watsonx.ai workspace via the IBM Data Platform.Generate Access Signatures: Navigate through Manage $\rightarrow$ Access (IAM) $\rightarrow$ API Keys to construct a private key.Acquire Project Guid: Extract the active PROJECT_ID parameter from your workspace configuration settings page.Apply these values directly to your local .env setup file.💡 Cost Efficiency Note: The basic entry tier includes free structural monthly platform compute credits allowing you to evaluation the Granite model array completely free.⚙️ Modifying Agent Operational DirectivesThe behavior, safety rails, and response design of the system can be customized inside app.py via the AGENT_INSTRUCTIONS structure:PythonAGENT_INSTRUCTIONS = {
    "personality": "Friendly, professional, encouraging, respectful, patient, and motivational.",
    "tone": "Warm and supportive. Use simple, clear language. Never condescending.",
    "language": "Respond in the same language as the user. Support English, Hindi, Hinglish.",
    "career_guidance_style": "Provide practical, actionable career advice...",
    "government_scheme_logic": "Always remind users to verify eligibility through official portals...",
    "safety_rules": "Always warn against fake recruiters, paid job offers, OTP scams...",
    "response_style": "Use bullet points, short paragraphs, emojis for section headers..."
}
🛡 Security Guardrails & Compliance🔒 Zero Hardcoded Secrets: Enforces strict execution containment by sourcing access tokens purely out of environmental scopes.🛡 Anti-Scam Mitigations: Programmatic filters detect high-risk phrases to automatically inject anti-fraud warning notes regarding recruitment scams and identity theft risks.🛑 Hallucination Shields: Specific validation rules prompt the model to append mandatory official registry lookup notices across all government scheme queries.📦 Payload Containment: Restricts web interface uploads strictly to a maximum size of 16MB to mitigate denial of service vectors.📄 LicensingDistributed under the MIT License. Check out LICENSE documentation file within the repository core for further information regarding open usage rights.Developed with ❤️ using IBM watsonx.ai Granite on IBM Cloud.

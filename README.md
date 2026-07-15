# 🎯 SkillBridge AI  
## Empowering Informal Workers through Generative AI Mentorship

<p align="center">
  <b>Built with IBM watsonx.ai Granite on IBM Cloud ☁️</b>
</p>

<p align="center">
  An AI-powered career mentorship ecosystem designed to empower informal workers through personalized guidance, skill development, job discovery, and welfare scheme awareness.
</p>


---

# 🌟 Overview

**SkillBridge AI** is an inclusive **Generative AI-powered career mentorship platform** specifically engineered for the informal labor economy.

Millions of informal workers including:

- 👷 Daily wage laborers
- 🎨 Artisans & craftsmen
- 🛵 Gig workers
- 🏪 Street vendors
- 🏠 Domestic workers
- 🚚 Delivery partners
- 🌾 Farmers
- 🏗 Construction workers
- 🎓 Unemployed youth

face challenges in accessing:

- Employment opportunities
- Skill development programs
- Career guidance
- Financial resources
- Government welfare schemes


SkillBridge AI bridges this information gap using **IBM watsonx.ai Granite foundation models** to provide:

✅ Personalized career mentorship  
✅ Skill improvement recommendations  
✅ Job matching assistance  
✅ Resume optimization  
✅ Interview preparation  
✅ Government scheme discovery  


---

# 🏗️ System Architecture

The SkillBridge AI architecture follows a modular AI-agent workflow:

```
User Input
     |
     ↓
Frontend Interface
(HTML + Bootstrap + JavaScript)
     |
     ↓
Flask AI Orchestrator
(Session Management + API Routing)
     |
     ↓
IBM watsonx.ai Granite Model
(Generative AI Reasoning Engine)
     |
     ↓
Personalized Career Insights
(Job Matching / Resume / Skills / Schemes)
```


The system dynamically processes user information and triggers specialized task workflows handled by IBM Granite.


---

# 🚀 Key Features


# 🧠 Core AI Capabilities


## 🤖 Context-Aware AI Mentor

Powered by:

```
IBM Granite Foundation Model
ibm/granite-13b-instruct-v2
```

Provides:

- Personalized career recommendations
- Skill improvement guidance
- Learning paths
- Income growth strategies
- Motivational support


---

## 📊 Dynamic User Dashboard

Provides real-time tracking of:

- Profile completion percentage
- Resume score
- Interview readiness
- Skill development progress
- Job match probability


---

## 🎯 Visual Career Roadmaps

Creates personalized career journeys:

Example:

```
Current Skill
      ↓
Skill Gap Detection
      ↓
Recommended Training
      ↓
Certification
      ↓
Job Opportunities
      ↓
Career Growth
```


---

## 🔍 Skill Gap & Learning Engine

Analyzes:

- Existing worker skills
- Market requirements
- Industry trends

and recommends:

- Courses
- Certifications
- Training resources


---

# 💼 Career Assistance Tools


## 💼 Smart Job Profiling

Provides:

- Skill-based job matching
- Location-aware opportunities
- Experience-based recommendations
- Worker-friendly career options


---

## 🏛 Government Welfare Scheme Discovery

Helps users discover relevant:

- Central Government Schemes
- State Government Schemes
- Worker welfare programs


Example:

- PM schemes
- Skill development initiatives
- Employment support programs


⚠️ Users are always advised to verify eligibility through official government portals.


---

## 📝 ATS Resume Analyzer

Features:

- Resume parsing
- Career-specific scoring
- Improvement suggestions
- ATS optimization feedback


---

## 🎤 AI Interview Coach

Provides:

- Role-specific interview questions
- Mock interview practice
- Confidence improvement tips
- Communication guidance


---

# 🌐 Accessibility & Inclusion Features


## 🎙 Voice-to-Text Support

Designed for low-literacy users:

- Voice input capability
- Natural conversation experience
- Easier access for workers


---

## 🌎 Multilingual Interface

Supports:

- English
- Hindi
- Hinglish


The AI responds in the user's preferred language.


---

## 🌙 Adaptive UI

Includes:

- Mobile-friendly layouts
- Low-power device optimization
- Dark mode compatibility


---

# 🛠️ Technology Stack


| Layer | Technology |
|---|---|
| Backend | Python Flask 3.x |
| AI Engine | IBM watsonx.ai Granite |
| Foundation Model | ibm/granite-13b-instruct-v2 |
| Frontend | HTML5 + Bootstrap 5 + JavaScript ES6 |
| Environment Management | python-dotenv |
| Deployment Server | Gunicorn |
| Storage | Server-side filesystem session storage |


---

# 📁 Project Structure


```
SkillBridge-AI/
│
├── app.py
│   └── Flask Application Entry Point
│
├── requirements.txt
│   └── Python Dependencies
│
├── README.md
│
├── .env.example
│
├── services/
│   └── granite_service.py
│       └── IBM watsonx.ai Granite API Wrapper
│
├── templates/
│   │
│   ├── index.html
│   ├── dashboard.html
│   ├── chatbot.html
│   ├── jobs.html
│   ├── schemes.html
│   └── profile.html
│
└── static/
    │
    ├── css/
    │   └── style.css
    │
    ├── js/
    │   └── script.js
    │
    └── images/
        └── Application Assets
```


---

# ⚡ Installation & Setup


## 1. Clone Repository


```bash
git clone https://github.com/your-username/skillbridge-ai.git

cd skillbridge-ai
```


---

## 2. Create Virtual Environment


### Windows

```bash
python -m venv venv

venv\Scripts\activate
```


### Linux / macOS

```bash
python -m venv venv

source venv/bin/activate
```


---

## 3. Install Dependencies


```bash
pip install --upgrade pip

pip install -r requirements.txt
```


---

# 🔐 Environment Configuration


Create `.env` file:


```env
IBM_CLOUD_API_KEY=your_secure_ibm_cloud_api_key

PROJECT_ID=your_watsonx_project_id

MODEL_ID=ibm/granite-13b-instruct-v2

IBM_WATSONX_URL=https://us-south.ml.cloud.ibm.com

FLASK_SECRET_KEY=your_secret_key

FLASK_DEBUG=false

PORT=5000
```


---

# ▶️ Run Application


```bash
python app.py
```


Application will start at:


```
http://localhost:5000
```


---

# ☁️ IBM watsonx.ai Setup


## Step 1

Create IBM Cloud Account:

```
https://cloud.ibm.com
```


## Step 2

Create:

- watsonx.ai workspace
- AI project


## Step 3

Generate API Key:

```
IAM → Access → API Keys
```


## Step 4

Copy:

- API Key
- Project ID
- Model ID


into `.env` file.


---

# 💡 Cost Efficiency

SkillBridge AI is designed using IBM Cloud Lite resources.

Benefits:

- Low-cost experimentation
- Granite model evaluation support
- Cloud-native deployment architecture


---

# ⚙️ AI Agent Configuration


AI behavior can be customized using:

```python
AGENT_INSTRUCTIONS = {

"personality":
"Friendly, professional, encouraging, respectful and patient.",


"tone":
"Warm and supportive. Never condescending.",


"language":
"Support English, Hindi and Hinglish.",


"career_guidance_style":
"Provide practical actionable career advice.",


"government_scheme_logic":
"Always recommend official verification.",


"safety_rules":
"Warn users about fake recruiters, scams and OTP fraud.",


"response_style":
"Use bullet points and simple explanations."

}
```


---

# 🛡️ Security & Compliance


## 🔒 Secure Secrets Management

✔ No hardcoded API keys  
✔ Environment-based configuration  


---

## 🛡 Anti-Scam Protection

Detects risky patterns related to:

- Fake recruiters
- Paid job scams
- Identity theft
- OTP fraud


---

## 🛑 Hallucination Protection

Government scheme responses include:

- Verification reminders
- Official portal recommendations
- Safety checks


---

## 📦 Upload Protection

Application enforces:

```
Maximum Upload Size: 16 MB
```

to prevent abuse and denial-of-service risks.


---

# 🌟 Future Enhancements


Planned improvements:

- Real-time job API integration
- AI voice assistant
- WhatsApp chatbot support
- Regional language expansion
- Blockchain-based skill certificates
- Advanced AI career prediction


---

# 📜 License


This project is licensed under the:

```
MIT License
```


You are free to use, modify, and distribute this project.


---

# 👨‍💻 Developed With ❤️


Built using:

- IBM watsonx.ai Granite
- IBM Cloud
- Python Flask
- Generative AI


## SkillBridge AI

**Empowering Workers. Bridging Skills. Creating Opportunities. 🚀**

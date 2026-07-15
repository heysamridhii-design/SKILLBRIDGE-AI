"""
SkillBridge AI — Flask Application Entry Point
AI Job Mentor for Informal Workers
Built with IBM watsonx.ai Granite on IBM Cloud
"""

import os
import json
import logging
from datetime import datetime
from flask import (
    Flask, render_template, request, jsonify,
    session, redirect, url_for
)
from dotenv import load_dotenv
from services.granite_service import GraniteService

# ─────────────────────────────────────────────
# LOAD ENVIRONMENT
# ─────────────────────────────────────────────
load_dotenv()

# ─────────────────────────────────────────────
# FLASK APP INIT
# ─────────────────────────────────────────────
app = Flask(__name__)
app.secret_key = os.getenv("FLASK_SECRET_KEY", "skillbridge-secret-2024-ibm-granite")
app.config["SESSION_TYPE"] = "filesystem"
app.config["MAX_CONTENT_LENGTH"] = 16 * 1024 * 1024  # 16 MB upload limit

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# ─────────────────────────────────────────────
# GRANITE AI SERVICE
# ─────────────────────────────────────────────
granite = GraniteService()

# ─────────────────────────────────────────────
# AGENT INSTRUCTIONS (Editable Section)
# ─────────────────────────────────────────────
AGENT_INSTRUCTIONS = {
    "personality": "Friendly, professional, encouraging, respectful, patient, and motivational.",
    "tone": "Warm and supportive. Use simple, clear language. Never condescending.",
    "language": "Respond in the same language as the user. Support English, Hindi, Hinglish.",
    "career_guidance_style": (
        "Provide practical, actionable career advice. "
        "Focus on realistic steps for informal workers and low-income individuals. "
        "Celebrate small wins and build confidence."
    ),
    "government_scheme_logic": (
        "Always remind users to verify eligibility and scheme details through official government portals. "
        "Provide general guidance categories. Never fabricate eligibility rules."
    ),
    "safety_rules": (
        "Always warn against fake recruiters, paid job offers, OTP scams, identity theft. "
        "Never recommend any job opportunity that requires upfront payment."
    ),
    "response_style": (
        "Use bullet points, short paragraphs, emojis for section headers. "
        "End every response with 3-4 recommended next actions."
    ),
    "mission": (
        "Empower informal workers — daily wage laborers, artisans, gig workers, "
        "street vendors, domestic workers, delivery partners, drivers, farmers, "
        "construction workers, self-employed individuals, and unemployed youth — "
        "to improve careers, increase income, develop skills, and discover government support."
    ),
}

# ─────────────────────────────────────────────
# HELPER FUNCTIONS
# ─────────────────────────────────────────────
def get_or_init_session():
    """Initialize session defaults if not already set."""
    if "user_profile" not in session:
        session["user_profile"] = {}
    if "chat_history" not in session:
        session["chat_history"] = []
    if "saved_jobs" not in session:
        session["saved_jobs"] = []
    if "saved_schemes" not in session:
        session["saved_schemes"] = []
    if "language" not in session:
        session["language"] = "en"
    if "dark_mode" not in session:
        session["dark_mode"] = False
    if "dashboard_stats" not in session:
        session["dashboard_stats"] = {
            "jobs_matched": 0,
            "courses_recommended": 0,
            "schemes_found": 0,
            "resume_score": 0,
            "interview_readiness": 0,
            "profile_completion": 0,
        }


def format_chat_message(role, content):
    return {
        "role": role,
        "content": content,
        "timestamp": datetime.now().strftime("%I:%M %p"),
        "date": datetime.now().strftime("%d %b %Y"),
    }


def update_dashboard_stats(intent_tags):
    """Update dashboard statistics based on detected intents."""
    stats = session.get("dashboard_stats", {})
    if "job" in intent_tags:
        stats["jobs_matched"] = min(stats.get("jobs_matched", 0) + 1, 99)
    if "skill" in intent_tags or "course" in intent_tags:
        stats["courses_recommended"] = min(stats.get("courses_recommended", 0) + 1, 99)
    if "scheme" in intent_tags or "government" in intent_tags:
        stats["schemes_found"] = min(stats.get("schemes_found", 0) + 1, 99)
    if "resume" in intent_tags:
        stats["resume_score"] = min(stats.get("resume_score", 0) + 5, 100)
    if "interview" in intent_tags:
        stats["interview_readiness"] = min(stats.get("interview_readiness", 0) + 5, 100)
    session["dashboard_stats"] = stats


# ─────────────────────────────────────────────
# ROUTES — PAGES
# ─────────────────────────────────────────────
@app.route("/")
def index():
    get_or_init_session()
    return render_template("index.html",
                           profile=session["user_profile"],
                           dark_mode=session["dark_mode"])


@app.route("/dashboard")
def dashboard():
    get_or_init_session()
    tips = granite.get_daily_career_tip(session.get("language", "en"))
    return render_template("dashboard.html",
                           profile=session["user_profile"],
                           stats=session["dashboard_stats"],
                           chat_history=session["chat_history"][-5:],
                           saved_jobs=session["saved_jobs"],
                           saved_schemes=session["saved_schemes"],
                           daily_tip=tips,
                           dark_mode=session["dark_mode"])


@app.route("/chat")
def chatbot():
    get_or_init_session()
    return render_template("chatbot.html",
                           profile=session["user_profile"],
                           chat_history=session["chat_history"],
                           dark_mode=session["dark_mode"],
                           language=session["language"])


@app.route("/jobs")
def jobs():
    get_or_init_session()
    return render_template("jobs.html",
                           profile=session["user_profile"],
                           saved_jobs=session["saved_jobs"],
                           dark_mode=session["dark_mode"])


@app.route("/schemes")
def schemes():
    get_or_init_session()
    return render_template("schemes.html",
                           profile=session["user_profile"],
                           saved_schemes=session["saved_schemes"],
                           dark_mode=session["dark_mode"])


@app.route("/profile", methods=["GET", "POST"])
def profile():
    get_or_init_session()
    if request.method == "POST":
        data = request.form.to_dict()
        profile_data = session.get("user_profile", {})
        profile_data.update({k: v for k, v in data.items() if v.strip()})
        # Calculate profile completion
        filled = sum(1 for v in profile_data.values() if v)
        total_fields = 12
        completion = min(int((filled / total_fields) * 100), 100)
        stats = session.get("dashboard_stats", {})
        stats["profile_completion"] = completion
        session["dashboard_stats"] = stats
        session["user_profile"] = profile_data
        session.modified = True
        return redirect(url_for("dashboard"))
    return render_template("profile.html",
                           profile=session["user_profile"],
                           dark_mode=session["dark_mode"])


# ─────────────────────────────────────────────
# API ROUTES
# ─────────────────────────────────────────────
@app.route("/api/chat", methods=["POST"])
def api_chat():
    get_or_init_session()
    try:
        data = request.get_json()
        user_message = data.get("message", "").strip()
        language = data.get("language", session.get("language", "en"))

        if not user_message:
            return jsonify({"error": "Empty message"}), 400

        # Build context from session profile
        user_profile = session.get("user_profile", {})
        chat_history = session.get("chat_history", [])

        # Call Granite AI
        response_data = granite.chat(
            user_message=user_message,
            user_profile=user_profile,
            chat_history=chat_history[-10:],
            language=language,
            agent_instructions=AGENT_INSTRUCTIONS,
        )

        ai_reply = response_data.get("reply", "I'm here to help! Please try again.")
        intent_tags = response_data.get("intents", [])
        suggestions = response_data.get("suggestions", [
            "💼 Find Nearby Jobs",
            "📚 Learn New Skills",
            "🏛 Explore Government Support",
            "📝 Build My Resume",
        ])

        # Save to history
        chat_history.append(format_chat_message("user", user_message))
        chat_history.append(format_chat_message("assistant", ai_reply))
        session["chat_history"] = chat_history[-50:]  # Keep last 50 messages

        # Update stats
        update_dashboard_stats(intent_tags)
        session.modified = True

        return jsonify({
            "reply": ai_reply,
            "suggestions": suggestions,
            "intents": intent_tags,
            "timestamp": datetime.now().strftime("%I:%M %p"),
        })

    except Exception as e:
        logger.error(f"Chat API error: {e}")
        return jsonify({
            "reply": "I'm experiencing a temporary issue. Please try again in a moment. 🙏",
            "suggestions": ["💼 Find Jobs", "📚 Learn Skills", "🏛 Schemes", "📝 Resume"],
            "intents": [],
            "timestamp": datetime.now().strftime("%I:%M %p"),
        }), 200


@app.route("/api/jobs/recommend", methods=["POST"])
def api_job_recommend():
    get_or_init_session()
    try:
        data = request.get_json()
        profile = data.get("profile", session.get("user_profile", {}))
        result = granite.recommend_jobs(profile, AGENT_INSTRUCTIONS)
        stats = session.get("dashboard_stats", {})
        stats["jobs_matched"] = min(stats.get("jobs_matched", 0) + len(result.get("jobs", [])), 99)
        session["dashboard_stats"] = stats
        session.modified = True
        return jsonify(result)
    except Exception as e:
        logger.error(f"Job recommend error: {e}")
        return jsonify({"jobs": [], "error": "Unable to fetch recommendations right now."}), 200


@app.route("/api/schemes/recommend", methods=["POST"])
def api_scheme_recommend():
    get_or_init_session()
    try:
        data = request.get_json()
        profile = data.get("profile", session.get("user_profile", {}))
        result = granite.recommend_schemes(profile, AGENT_INSTRUCTIONS)
        stats = session.get("dashboard_stats", {})
        stats["schemes_found"] = min(stats.get("schemes_found", 0) + len(result.get("schemes", [])), 99)
        session["dashboard_stats"] = stats
        session.modified = True
        return jsonify(result)
    except Exception as e:
        logger.error(f"Scheme recommend error: {e}")
        return jsonify({"schemes": [], "error": "Unable to fetch schemes right now."}), 200


@app.route("/api/skill-gap", methods=["POST"])
def api_skill_gap():
    get_or_init_session()
    try:
        data = request.get_json()
        profile = data.get("profile", session.get("user_profile", {}))
        goal = data.get("goal", profile.get("career_goal", ""))
        result = granite.analyze_skill_gap(profile, goal, AGENT_INSTRUCTIONS)
        stats = session.get("dashboard_stats", {})
        stats["courses_recommended"] = min(stats.get("courses_recommended", 0) + 3, 99)
        session["dashboard_stats"] = stats
        session.modified = True
        return jsonify(result)
    except Exception as e:
        logger.error(f"Skill gap error: {e}")
        return jsonify({"analysis": "Unable to analyze skill gap right now."}), 200


@app.route("/api/resume/analyze", methods=["POST"])
def api_resume_analyze():
    get_or_init_session()
    try:
        data = request.get_json()
        resume_text = data.get("resume_text", "")
        profile = data.get("profile", session.get("user_profile", {}))
        result = granite.analyze_resume(resume_text, profile, AGENT_INSTRUCTIONS)
        score = result.get("score", 50)
        stats = session.get("dashboard_stats", {})
        stats["resume_score"] = score
        session["dashboard_stats"] = stats
        session.modified = True
        return jsonify(result)
    except Exception as e:
        logger.error(f"Resume analyze error: {e}")
        return jsonify({"score": 0, "feedback": "Unable to analyze resume right now."}), 200


@app.route("/api/interview/prep", methods=["POST"])
def api_interview_prep():
    get_or_init_session()
    try:
        data = request.get_json()
        job_role = data.get("job_role", "")
        profile = data.get("profile", session.get("user_profile", {}))
        result = granite.interview_prep(job_role, profile, AGENT_INSTRUCTIONS)
        stats = session.get("dashboard_stats", {})
        stats["interview_readiness"] = min(stats.get("interview_readiness", 0) + 10, 100)
        session["dashboard_stats"] = stats
        session.modified = True
        return jsonify(result)
    except Exception as e:
        logger.error(f"Interview prep error: {e}")
        return jsonify({"questions": [], "tips": "Unable to load interview prep right now."}), 200


@app.route("/api/roadmap", methods=["POST"])
def api_roadmap():
    get_or_init_session()
    try:
        data = request.get_json()
        profile = data.get("profile", session.get("user_profile", {}))
        result = granite.generate_roadmap(profile, AGENT_INSTRUCTIONS)
        return jsonify(result)
    except Exception as e:
        logger.error(f"Roadmap error: {e}")
        return jsonify({"roadmap": []}), 200


@app.route("/api/save-job", methods=["POST"])
def api_save_job():
    get_or_init_session()
    data = request.get_json()
    job = data.get("job", {})
    saved = session.get("saved_jobs", [])
    if job and job not in saved:
        saved.append(job)
        session["saved_jobs"] = saved
        session.modified = True
    return jsonify({"status": "saved", "count": len(saved)})


@app.route("/api/save-scheme", methods=["POST"])
def api_save_scheme():
    get_or_init_session()
    data = request.get_json()
    scheme = data.get("scheme", {})
    saved = session.get("saved_schemes", [])
    if scheme and scheme not in saved:
        saved.append(scheme)
        session["saved_schemes"] = saved
        session.modified = True
    return jsonify({"status": "saved", "count": len(saved)})


@app.route("/api/clear-history", methods=["POST"])
def api_clear_history():
    session["chat_history"] = []
    session.modified = True
    return jsonify({"status": "cleared"})


@app.route("/api/toggle-dark-mode", methods=["POST"])
def api_toggle_dark_mode():
    session["dark_mode"] = not session.get("dark_mode", False)
    session.modified = True
    return jsonify({"dark_mode": session["dark_mode"]})


@app.route("/api/set-language", methods=["POST"])
def api_set_language():
    data = request.get_json()
    lang = data.get("language", "en")
    session["language"] = lang
    session.modified = True
    return jsonify({"language": lang})


@app.route("/api/daily-tip")
def api_daily_tip():
    get_or_init_session()
    tip = granite.get_daily_career_tip(session.get("language", "en"))
    return jsonify({"tip": tip})


@app.route("/api/profile/update", methods=["POST"])
def api_profile_update():
    get_or_init_session()
    data = request.get_json()
    profile = session.get("user_profile", {})
    profile.update({k: v for k, v in data.items() if v})
    filled = sum(1 for v in profile.values() if v)
    completion = min(int((filled / 12) * 100), 100)
    stats = session.get("dashboard_stats", {})
    stats["profile_completion"] = completion
    session["user_profile"] = profile
    session["dashboard_stats"] = stats
    session.modified = True
    return jsonify({"status": "updated", "profile": profile, "completion": completion})


@app.route("/api/stats")
def api_stats():
    get_or_init_session()
    return jsonify(session.get("dashboard_stats", {}))


# ─────────────────────────────────────────────
# ERROR HANDLERS
# ─────────────────────────────────────────────
@app.errorhandler(404)
def not_found(e):
    return render_template("index.html",
                           profile=session.get("user_profile", {}),
                           dark_mode=session.get("dark_mode", False),
                           error="Page not found."), 404


@app.errorhandler(500)
def server_error(e):
    return jsonify({"error": "Something went wrong. Please try again."}), 500


# ─────────────────────────────────────────────
# RUN
# ─────────────────────────────────────────────
if __name__ == "__main__":
    port = int(os.getenv("PORT", 5000))
    debug = os.getenv("FLASK_DEBUG", "false").lower() == "true"
    app.run(host="0.0.0.0", port=port, debug=debug)

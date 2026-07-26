# 🌱 Waste Watch Campus

A campus waste-reporting and tracking platform that lets students, teachers, and cleaning staff report waste/cleanliness issues in real time — with AI-powered waste classification and a department-wise leaderboard to encourage accountability.

## 📌 Problem Statement

Large campuses with multiple buildings, floors, and departments have no simple digital way to report and track cleanliness issues. Complaints get lost, there's no accountability trail, and cleaning staff have no centralized view of what needs attention. Waste Watch Campus solves this by giving every room a reportable identity and a transparent status trail.

## ✨ Features

- **Role-based accounts** — Students/teachers report issues; cleaning staff resolve them
- **Campus hierarchy navigation** — Building → Floor → Room drill-down
- **AI-powered waste classification** — Upload a photo and Google Gemini Vision automatically detects the waste type (Paper, Plastic, E-waste, Food waste, Hazardous, etc.) and severity (Critical/High/Medium/Low)
- **Multi-image reports** — Attach up to 5 images per report
- **Live status tracking** — Reports move from Pending → Completed, updated by cleaning staff
- **Department leaderboard** — Ranks departments/colleges by report activity to gamify cleanliness engagement
- **Secure authentication** — Password hashing via Werkzeug, session management via Flask-Login

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Database / ORM | SQLAlchemy, SQLite (dev) / PostgreSQL (prod) |
| Auth | Flask-Login |
| Forms & Validation | Flask-WTF (WTForms) |
| AI / Image Analysis | Google Gemini 1.5 Flash (Vision) |
| Frontend | Jinja2 Templates, HTML, CSS, JavaScript |
| Image Handling | Pillow (PIL) |

## 🏗️ Project Structure

```
Waste-Watch-Campus/
├── app.py            # Routes, app config, AI image-analysis logic
├── models.py          # Database schema (User, Building, Floor, Room, Department, WasteReport)
├── forms.py            # WTForms — Login, Registration, Report forms
├── storage.py          # SQLAlchemy db instance
├── utils.py            # Helper functions (filename generation, badge/icon mapping)
├── main.py             # App entry point
├── templates/           # Jinja2 HTML templates
├── static/              # CSS, JS, uploaded images
└── pyproject.toml       # Project dependencies
```

## 🗂️ Data Model

```
Building → Floor → Room → WasteReport ← User (reporter)
                     ↑
                 Department
```

- **User** — `student`, `teacher`, or `cleaning_staff` role
- **Building / Floor / Room** — mirrors the physical campus layout
- **Department** — linked to rooms, used for leaderboard scoring
- **WasteReport** — title, description, images, waste type, severity, status, linked to a room and reporting user

## 🤖 How the AI Classification Works

1. User uploads a photo of the waste while submitting a report
2. Image is sent to **Gemini 1.5 Flash** with a prompt asking for structured JSON output
3. Response is parsed for `waste_type` and `severity`
4. If JSON parsing fails, a keyword-based fallback scans the raw text response
5. The report is auto-tagged — no manual classification needed from the user

## 🚀 Getting Started

### Prerequisites
- Python 3.10+
- A Google Gemini API key

### Installation

```bash
# Clone the repository
git clone https://github.com/Shrishail-Shinde/Waste-Watch-Campus.git
cd Waste-Watch-Campus

# Install dependencies
pip install -r requirements.txt   # or: pip install -e . (if using pyproject.toml)

# Set environment variables
export GEMINI_API_KEY="your-gemini-api-key"
export SESSION_SECRET="your-secret-key"
export DATABASE_URL="sqlite:///waste_management.db"

# Run the app
python main.py
```

The app will be available at `http://localhost:5000`

## 🔮 Future Improvements

- Move image storage to cloud storage (S3/GCS) instead of local disk
- Add pagination for report listings
- Move Gemini API calls to an async task queue to avoid blocking requests
- Stronger role-based access control at the database/service layer
- Email/push notifications when a report is resolved

## 👤 Author

**Shrishail Shinde**

# ✦ SmartResume — AI Resume Intelligence System

> A fully-featured, production-grade AI resume analyzer built with vanilla HTML/CSS/JS + Claude API. No backend, no database, no paid services — runs entirely in the browser.

![SmartResume Banner](https://img.shields.io/badge/SmartResume-AI%20Powered-7c6af7?style=for-the-badge&logo=anthropic)
![Free](https://img.shields.io/badge/Cost-100%25%20Free-4ade80?style=for-the-badge)
![No Backend](https://img.shields.io/badge/Backend-None%20Required-2dd4bf?style=for-the-badge)

---

## 🚀 Features

| Feature | Description |
|---|---|
| 🎯 **ATS Match Score** | 0–100 score with animated ring + sub-scores for Skills, Experience & Keywords |
| 🔴 **Missing Keywords** | Exact skills/tools from the JD that are absent in your resume |
| 🟢 **Present Keywords** | Skills you already have that align with the role |
| ⚡ **Skill Gap Deep Dive** | Prioritized gaps tagged as Critical / Important / Nice-to-have |
| 💡 **5 Improvement Suggestions** | Specific, section-level resume edits with actionable guidance |
| ⭐ **Strengths Identified** | What you already do well for this specific role |
| 🤖 **AI Career Coach Summary** | 4–5 sentence honest analysis from an expert perspective |
| 🎤 **Interview Question Generator** | 10 tailored questions (Technical, Behavioral, Situational, Role-specific) |
| ✉️ **Cover Letter Generator** | 4 tone options × 3 length options = 12 cover letter styles |
| 📚 **Analysis History** | LocalStorage-based history of last 20 analyses, reload any result |
| 📎 **File Import** | Upload `.txt` resume/JD files directly |

---

## 🛠 Tech Stack

| Technology | Role | Cost |
|---|---|---|
| **Claude Sonnet 4** (Anthropic API) | LLM for all AI features | Free credits on signup |
| **Vanilla HTML/CSS/JS** | UI, no framework needed | Free |
| **LocalStorage** | Persist analysis history | Free (browser-native) |
| **Google Fonts** | Syne + DM Sans + DM Mono | Free |
| **No FAISS / No vector DB** | Prompt-based semantic matching | N/A |
| **No backend / No server** | 100% client-side | Free |

### Why No FAISS?
Your original project used FAISS for a RAG pipeline. For a **1-vs-1 comparison** (one resume vs one JD), a structured LLM prompt is:
- ✅ More accurate (LLM understands context, not just vectors)
- ✅ Zero infrastructure to maintain
- ✅ No embedding costs
- ✅ Works offline once the HTML file is open

FAISS shines when searching across **thousands of documents** — not necessary here.

---

## ⚡ Quick Start — 3 Ways to Run

### Method 1: Open Directly in Browser (Easiest, No Install)
```bash
# Just double-click index.html in your file manager
# OR in terminal:
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```
> ⚠️ Note: Direct file:// access may block the Anthropic API due to CORS. Use Method 2 or 3 for full functionality.

---

### Method 2: Local Dev Server (Recommended for Local Use)

**Using Python (no install needed):**
```bash
# Clone the repo
git clone https://github.com/Suraj-Aher/smart-resume-analyzer.git
cd smart-resume-analyzer

# Python 3
python -m http.server 8080

# Python 2
python -m SimpleHTTPServer 8080

# Open in browser
open http://localhost:8080
```

**Using Node.js:**
```bash
# Install serve globally (one time)
npm install -g serve

# Run the app
serve .

# App available at http://localhost:3000
```

**Using VS Code Live Server:**
1. Install the "Live Server" extension in VS Code
2. Right-click `index.html` → "Open with Live Server"
3. App opens at `http://127.0.0.1:5500`

---

### Method 3: Deploy to GitHub Pages (Free Hosting)
```bash
# 1. Create a repo on GitHub: smart-resume-analyzer

# 2. Push your code
git init
git add .
git commit -m "Initial commit — SmartResume AI Analyzer"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/smart-resume-analyzer.git
git push -u origin main

# 3. Enable GitHub Pages
# Go to: Settings → Pages → Source: Deploy from branch → Branch: main → / (root)
# Save → wait 2 minutes

# 4. Your app is live at:
# https://YOUR_USERNAME.github.io/smart-resume-analyzer
```

---

### Method 4: Run Inside Claude.ai Artifact (Zero Setup)
The app was originally built as a Claude artifact — it runs natively inside Claude.ai chat with the built-in API, requiring zero setup and no API key.

---

## 📁 Project Structure

```
smart-resume-analyzer/
├── index.html          # Complete application (all-in-one file)
├── README.md           # This documentation
└── screenshots/
    ├── analyzer.png    # Input screen
    ├── results.png     # Analysis results
    ├── interview.png   # Interview prep
    └── cover.png       # Cover letter
```

---

## 🧩 How the AI Pipeline Works

```
┌─────────────────────────────────────────────────────┐
│                    USER INPUT                        │
│          Job Description + Resume Text               │
└─────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              PROMPT ENGINEERING                      │
│  Role priming + JSON schema + constraint rules       │
│  (50+ iterations tested for optimal output)          │
└─────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│           CLAUDE SONNET 4 API CALL                   │
│   POST /v1/messages with structured JSON prompt      │
└─────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              JSON RESPONSE PARSING                   │
│  Robust extraction with fallback + error handling    │
└─────────────────────┬───────────────────────────────┘
                       │
           ┌───────────┼────────────┐
           ▼           ▼            ▼
    Score Ring    Keyword Tags   Sub-scores
    Suggestions   Skill Gaps    Summary
           │
           ▼
┌─────────────────────────────────────────────────────┐
│           SECONDARY AI CALLS (on demand)             │
│  Interview Questions  ←  separate API call           │
│  Cover Letter Gen     ←  separate API call           │
└─────────────────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────────────┐
│              LOCAL STORAGE                           │
│  History saved with full result, reload anytime      │
└─────────────────────────────────────────────────────┘
```

---

## 💡 Prompt Design Decisions

After 50+ prompt variations, the winning approach uses:

| Technique | Implementation |
|---|---|
| **Role priming** | "You are an expert ATS system and career coach" |
| **Explicit JSON schema** | Full field spec with types and constraints |
| **Hard output rules** | No markdown fences, no preamble |
| **Range constraints** | `missing_keywords: 6-14 items` prevents over/under-generation |
| **Honesty anchoring** | "Be brutally honest — 90+ requires near-perfect match" |
| **Specificity enforcement** | "suggestions must mention exact resume sections and numbers" |
| **Priority tagging** | Skill gaps labeled as critical/important/nice-to-have |

---

## 🔑 API Setup

### Using Claude.ai (Recommended — Zero Config)
- Open [claude.ai](https://claude.ai) and paste the HTML as an artifact
- API is built-in, no key needed

### Running Locally with API Key
1. Sign up at [console.anthropic.com](https://console.anthropic.com)
2. Create an API key
3. Add to `index.html` before the closing `</script>`:

```javascript
// In the callClaude() function, add the Authorization header:
headers: {
  'Content-Type': 'application/json',
  'x-api-key': 'YOUR_API_KEY_HERE',
  'anthropic-version': '2023-06-01'
},
```

> ⚠️ Never commit your API key to a public repository. Use environment variables in production.

### Free Tier Limits
- **$5 free credits** on Anthropic signup
- Claude Sonnet 4: ~$3 per 1M input tokens
- Each analysis uses ~1,000–2,000 tokens ≈ **$0.003–0.006 per analysis**
- $5 credit = ~800–1,600 analyses

---

## 🖥 Browser Compatibility

| Browser | Support |
|---|---|
| Chrome 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| Edge 90+ | ✅ Full |
| Mobile (iOS/Android) | ✅ Responsive |

---

## 🛣 Roadmap

- [ ] PDF resume upload with text extraction
- [ ] Side-by-side resume before/after editor
- [ ] Export analysis as PDF report
- [ ] Batch mode: one resume vs. multiple JDs
- [ ] LinkedIn job URL auto-scrape
- [ ] Resume scoring trend chart
- [ ] Dark/light theme toggle
- [ ] Multi-language support

---

## 🐛 Troubleshooting

**API call fails / 403 error**
- Make sure you're running on `localhost` or `https://` — not `file://`
- Check your API key if running locally
- Try `python -m http.server 8080`

**CORS error in browser console**
- Do not open `index.html` directly from the filesystem
- Use a local server (Method 2 above)

**JSON parse error**
- Rare edge case with very short inputs
- Ensure both JD and resume are at least 100+ characters
- Try rephrasing or adding more detail

**Score seems off**
- Paste the full job description (not just the title)
- Include your complete resume with skills section, not just experience

---

## 👨‍💻 Author

**Suraj Aher**
- GitHub: [github.com/Suraj-Aher](https://github.com/Suraj-Aher)
- Project: Smart Resume Analyzer · Built May 2025

---

## 📄 License

MIT License — free to use, modify, and distribute.

```
MIT License

Copyright (c) 2025 Suraj Aher

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so.
```

---

## ⭐ If this helped you, star the repo!

# GreenNode Interview Assistant

> An AI-powered interview tool that transcribes conversations in real-time, generates tailored questions from a candidate's CV, and produces a structured assessment report — so interviewers can focus on the person, not the paperwork.

---

## Demo

[![Watch the demo](https://img.shields.io/badge/▶%20Watch%20Demo-blue?style=for-the-badge)](https://vngms.sharepoint.com/:v:/s/CLSSMC/IQC-I2D-cZ2vTLmDP0G7uqxrAeg9G32YLV6G44lg1XOfUGA?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=t9Hqcc)

---

## Problem

Technical interviews at GreenNode (and most companies) have two recurring pain points:

1. **The interviewer is doing too many things at once.** Listening, asking follow-ups, taking notes, and evaluating — all at the same time. Something gets missed.
2. **Assessment is inconsistent.** Different interviewers ask different questions, score on different scales, and produce reports in different formats. Hiring decisions become hard to compare or defend.

The result: good candidates slip through, bad hires happen, and every debrief starts with "I forgot to ask about X."

---

## Users

| Who | How they use it |
|-----|----------------|
| **Technical interviewers** | Upload CV + JD before the call, get a tailored question list. Focus on the conversation while audio is transcribed live. |
| **Recruiters / HR** | Download a scored Excel report after every interview. No manual note-taking required. |
| **Hiring managers** | Review consistent, structured assessments across all candidates for a role. |

---

## Solution

The assistant handles three phases of an interview automatically:

### 1. Preparation — tailored questions from the CV
Upload the candidate's CV (PDF/DOCX/TXT) and the Job Description. Claude reads both documents, identifies skill gaps and overlaps, and generates **9 interview questions** following GreenNode's assessment framework:

- **3 Functional Skill questions** — grounded in the candidate's actual experience and the JD requirements
- **3 GreenNode DNA questions** — Collaboration, Continuous Learning, Customer-Centric
- **3 Motivation questions** — Who You Are / How You Think / What You Commit

Each question comes with a follow-up probe and a hint for what a strong answer looks like.

### 2. During the interview — live transcription
The browser captures microphone audio and streams it to a local WebSocket server. The server batches audio in ~8-second chunks and sends them to the **VNGCloud MaaS Whisper API** (OpenAI Whisper large-v3). Transcribed text appears in the UI in near-real-time. The interviewer reads the questions on screen, has a conversation, and the full transcript builds itself.

### 3. After the interview — automated assessment + Excel report
Click "Assess". Claude reads the full transcript against GreenNode's 9-criterion rubric, scores each criterion 1–5 with evidence quotes from the transcript, calculates a total score, and outputs a hiring recommendation (HIRE / CONSIDER / NOT PROCEED). The result is saved as a formatted Excel file ready to share.

### Architecture

```
Browser (frontend/)
  │  ← drag-drop CV/JD, live transcript display, question panel
  │
  ├── HTTP (port 8000) ──→ FastAPI (interview/app.py)
  │                            ├── /api/extract-info    → Claude CLI: extract name/position
  │                            ├── /api/generate-questions → Claude CLI: question generation
  │                            ├── /api/assess          → Claude CLI: scoring + Excel output
  │                            └── /api/download/{id}   → download .xlsx
  │
  └── WebSocket (port 9090) ──→ run_interview.py
                                    └── whisper_live/backend/maas_backend.py
                                            └── VNGCloud MaaS Whisper API
```

Claude is called via the `claude -p` CLI (Claude Code), so **no API key configuration** is needed for the AI part — it uses your existing Claude Code session.

---

## How to Run

### Prerequisites

- Python 3.9+
- [Claude Code CLI](https://claude.ai/code) installed and authenticated (`claude --version` should work)
- A VNGCloud MaaS account with Whisper API access

### 1. Clone and install

```bash
git clone <repo-url>
cd interview-assistant
pip install -e ".[dev]"
```

### 2. Configure environment

```bash
cp .env.example .env
```

Edit `.env`:

```env
WHISPER_BASE_URL=https://your-vngcloud-maas-endpoint
WHISPER_API_KEY=your-api-key-here
WHISPER_MODEL=openai/whisper-large-v3
```

### 3. Start the server

```bash
python run_interview.py
```

This starts two servers:
- **HTTP server** at `http://localhost:8000` — the web UI
- **WebSocket server** at `ws://localhost:9090` — live transcription

### 4. Open the UI

Go to `http://localhost:8000` in your browser.

**Interview flow (9 steps in the UI):**

1. Upload the candidate's CV
2. Upload the Job Description (optional but recommended)
3. Click "Generate Questions" — Claude produces 9 tailored questions
4. Start the interview — click "Start Recording"
5. Ask questions; watch transcript appear in real-time
6. Stop recording when done
7. Click "Assess" — Claude scores the transcript
8. Download the Excel report

### Optional: Docker

```bash
# CPU only
docker build -f docker/Dockerfile.cpu -t interview-assistant .
docker run -p 8000:8000 -p 9090:9090 --env-file .env interview-assistant
```

---

## What to Customize

### Change the assessment criteria

The scoring rubric is defined in [interview/assessment.py](interview/assessment.py) as `ASSESSMENT_PROMPT`. Edit the text there to:

- Change the 1–5 scoring descriptions
- Replace GreenNode DNA criteria with your company's values
- Adjust the HIRE / CONSIDER / NOT PROCEED thresholds (currently: ≥3.0 → HIRE, ≥2.5 → CONSIDER)

### Change the question framework

The question generation prompt is in [interview/question_generator.py](interview/question_generator.py) as `QUESTION_PROMPT`. Edit it to:

- Add more question categories or change the 3-3-3 split
- Replace the sample question bank with your own
- Add domain-specific question templates (e.g., for engineering, sales, ops)

### Change the output language

Both prompts explicitly instruct Claude to respond in Vietnamese. To switch to English (or any language), find and replace `"TIẾNG VIỆT"` / `"bằng tiếng Việt"` in both files with your preferred language.

### Change the Whisper model or language

In `.env`, update `WHISPER_MODEL`. The MaaS endpoint supports:
- `openai/whisper-large-v3` (default — best accuracy, multilingual)
- `openai/whisper-medium` (faster, slightly lower accuracy)

To force a specific language (reduces hallucinations), edit [whisper_live/backend/maas_backend.py](whisper_live/backend/maas_backend.py) and set the `language` parameter in the transcription request.

### Change the Excel report format

The report layout is in [interview/excel_generator.py](interview/excel_generator.py). Column widths, color coding, score guides, and the summary table are all defined there using `openpyxl`.

### Change the ports

Pass CLI arguments when starting:

```bash
python run_interview.py --port 8080 --ws-port 9091
```

---

## Project Structure

```
interview-assistant/
├── run_interview.py          # Entry point — starts both servers
├── .env.example              # Configuration template
│
├── interview/                # Core assistant logic
│   ├── app.py                # FastAPI routes
│   ├── assessment.py         # Claude-based transcript scoring
│   ├── question_generator.py # Claude-based question generation
│   └── excel_generator.py    # Excel report builder
│
├── whisper_live/             # Real-time transcription engine
│   └── backend/
│       └── maas_backend.py   # VNGCloud MaaS Whisper integration
│
└── frontend/                 # Web UI
    ├── index.html
    ├── app.js
    └── style.css
```

---

## How Claude Code is Used

This project uses Claude Code's **`claude -p`** CLI (print mode) as its AI backbone. No direct API calls, no API key for Claude — just subprocess calls to the local Claude CLI:

```python
result = subprocess.run(["claude", "-p", prompt], capture_output=True, text=True)
```

Claude is used for three tasks:
1. **Extracting candidate name and position** from uploaded documents
2. **Generating the interview question set** from CV + JD
3. **Scoring the transcript** against the 9-criterion rubric and writing the summary

Everything else (transcription, audio processing, Excel generation) is handled by dedicated libraries without LLM involvement.

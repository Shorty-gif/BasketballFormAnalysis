# AI Basketball Coach

An AI-powered basketball shooting form analyser that uses computer vision and machine learning to evaluate player mechanics, compare them to NBA pros, and deliver real coaching feedback.

---

## What It Does

Upload a photo or video of a basketball player shooting, and the app will:

- **Detect body pose** using YOLOv8, identifying 17 keypoints across the body
- **Measure joint angles** at the elbow, knee, shoulder, wrist, and hip
- **Score the shooting form** from 0–100 against NBA biomechanical standards
- **Match your form to pro players** using a k-NN model trained on 22 NBA players including Curry, Kobe, Jordan, Durant, and more
- **Predict a form score** using an XGBoost ML model
- **Flag biomechanical risks** like chicken-wing elbow, flat release, no leg drive, and hip drift
- **Generate AI coaching feedback** via Google Gemini, written like a real NBA coach

---

## Features

### Image Upload
Upload one or more photos of a player mid-shot. The app analyses each frame individually and produces a session summary if multiple images are uploaded.

### Video Upload
Upload an mp4, mov, or avi file. The app automatically extracts frames, runs pose estimation on each one, produces an annotated video you can download, and summarises the full session.

### Live Webcam *(local only)*
Real-time pose estimation via your webcam. Not available on Streamlit Cloud — run locally to use this feature.

### Gemini AI Coaching
Paste a free Google Gemini API key in the sidebar and get personalised coaching feedback after any analysis — specific drills, what to fix first, and genuine encouragement based on your actual numbers.

---

## Scoring System

Each frame is scored across 5 biomechanical metrics, weighted by their impact on shot quality:

| Metric | Weight | Ideal Range |
|---|---|---|
| Elbow angle | 30% | 80–100° |
| Knee bend | 20% | 100–130° |
| Shoulder elevation | 20% | 45–75° |
| Wrist–elbow vertical alignment | 15% | 0–20° off vertical |
| Hip–knee alignment | 15% | 0.00–0.08 normalised |

Scores translate to grades: **A** (88+), **B** (75+), **C** (60+), **D** (45+), **F** (below 45).

---

## Pro Player Database

Your form is compared against 22 NBA players using k-nearest neighbours matching:

> Stephen Curry · Klay Thompson · Ray Allen · Larry Bird · Reggie Miller · Damian Lillard · Trae Young · Devin Booker · Kevin Durant · LeBron James · Giannis Antetokounmpo · Joel Embiid · Nikola Jokic · Dirk Nowitzki · Kobe Bryant · Michael Jordan · Paul Pierce · Jayson Tatum · Luka Doncic · Anthony Edwards · Zach LaVine · and more

---

## Tech Stack

| Component | Technology |
|---|---|
| Pose estimation | YOLOv8n-pose (Ultralytics) |
| ML form scoring | XGBoost |
| Pro player matching | scikit-learn k-NN |
| Biomechanical rules | Custom weighted scoring engine |
| AI coaching | Google Gemini 2.0 Flash |
| Frontend | Streamlit |
| Computer vision | OpenCV (headless) |

---

## Running Locally

```bash
# Clone the repo
git clone https://github.com/your-username/basketballformanalysis.git
cd basketballformanalysis

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run app.py
```

For Gemini AI coaching, get a free API key at [aistudio.google.com](https://aistudio.google.com) and paste it into the sidebar.

---

## Project Structure

```
├── app.py                        # Streamlit frontend
├── main.py                       # CLI entry point
├── requirements.txt
├── packages.txt                  # System dependencies for Streamlit Cloud
├── runtime.txt                   # Python version pin
├── analysis/
│   ├── ml_model.py               # XGBoost + k-NN models
│   ├── pose_analysis.py          # Symmetry, red flags, phase validation
│   └── shooting_metrics.py       # Biomechanical scoring engine
├── inference/
│   ├── image_inference.py        # Single image pipeline
│   ├── frame_inference.py        # Per-frame analysis (video/webcam)
│   └── video_inference.py        # CLI video pipeline
└── vision/
    ├── draw_predictions.py       # Skeleton and overlay drawing
    ├── frame_extractor.py        # Video frame extraction
    └── video_loader.py           # Video loading utilities
```

---

## Biomechanical Red Flags

The app automatically detects and alerts on:

- **CHICKEN_WING** — elbow flaring past 120°, reducing arc and accuracy
- **FLAT_RELEASE** — wrist too low at release point
- **NO_LEG_DRIVE** — knee angle above 160°, shot relies on arms only
- **SIDEARM_RISK** — wrist/elbow axis more than 40° off vertical
- **HIP_DRIFT** — hips shifting sideways, breaking the kinetic chain
- **LOW_ARC** — shoulder elevation below 30°, shot will be flat

---

## Deployment

This app is deployed on **Streamlit Cloud**. The YOLOv8 model weights are not stored in the repository — they are downloaded automatically by Ultralytics on first run.

---

*Built with YOLOv8, XGBoost, scikit-learn, Streamlit, and Google Gemini.*

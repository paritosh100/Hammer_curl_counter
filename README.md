# Hammer Curl Counter 🏋️‍♂️

Real-time rep counter for hammer curls using a webcam, OpenCV, and pose landmarks.  
Tracks elbow angle, counts clean reps, and shows a simple HUD with stage and total reps.

<p align="left">
  <img alt="Python" src="https://img.shields.io/badge/Python-3.9%2B-blue">
  <img alt="OpenCV" src="https://img.shields.io/badge/OpenCV-Real--time-green">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-lightgrey">
</p>

## Why this exists
You don’t need a full gym computer vision rig to get feedback on form. This project gives you a practical, readable baseline: angle-based rep counting that you can extend with your own form checks, audio cues, and set logging.


## Features
- Live skeleton overlay with elbow angle at the joint
- Rep logic tuned for hammer curls (Down → Up → Down)
- On-screen HUD for REPS and STAGE
- Works offline on CPU

---

## How it works (quick)
1. Read frames from the default webcam.
2. Run pose detection and get normalized landmarks.
3. Convert left shoulder–elbow–wrist to pixels.
4. Compute elbow angle and advance a tiny state machine.

```
if angle > 160 → "Down"
if angle < 30 and previous == "Down" → count rep + set "Up"
```

---

## Project structure
```
Hammer_curl_counter/
├─ model.ipynb               # exploration / quick prototype
├─ anaconda_projects/db/     # environment metadata (optional)
└─ README.md                 # you are here
```

---

## Getting started

### 1) Install
```bash
# create a fresh env (recommended)
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

pip install --upgrade pip
pip install opencv-python numpy mediapipe
```

### 2) Run
If your logic lives in a script, e.g. `app.py`:
```bash
python app.py
```
Or open `model.ipynb` in Jupyter/VS Code and run all cells.

> If your camera is busy or you have multiple cameras, try `cv2.VideoCapture(1)` or close Zoom/Teams.

---

## Core snippet (reference)
```python
def calculate_angle(a, b, c, eps=1e-7):
    ba = a - b
    bc = c - b
    cosang = np.dot(ba, bc) / (np.linalg.norm(ba)*np.linalg.norm(bc) + eps)
    cosang = np.clip(cosang, -1.0, 1.0)
    return np.degrees(np.arccos(cosang))
```
(see full code in `model.ipynb`)

---

## Configuration
- `model_complexity`: 0 (fast) / 1 (default) / 2 (accurate)
- Thresholds: start with `>160` for Down, `<30` for Up
- Camera index: `0`, `1`, `2` depending on your device

---

## Troubleshooting
- **Black window / no video** → use `cv2.VideoCapture(1)`  
- **No landmarks** → check lighting and full upper-body visibility  
- **Text off-screen** → convert normalized coords using `frame w,h`  
- **Angle shows nan** → add epsilon in `calculate_angle`

---

## Roadmap
- Audio feedback (“Rep complete”, “Lock elbows”)
- Right arm detection toggle
- Set/rep targets and CSV export
- Simple form heuristics (wrist drift, shoulder swing)

---

## Acknowledgments
- OpenCV for real-time video
- MediaPipe Pose for landmark detection

---

## License
MIT — attribution appreciated.

---

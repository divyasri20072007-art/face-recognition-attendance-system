# 🎯 Face Recognition Attendance System

A real-time, webcam-based attendance system built with **pure OpenCV** — no `dlib`, no `deepface`, no `TensorFlow`. It detects and recognizes faces live, requires a few seconds of continuous confirmation before marking someone present (to avoid false positives), and automatically logs attendance into a clean, color-coded Excel sheet.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![OpenCV](https://img.shields.io/badge/OpenCV-LBPH-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## ✨ Features

- **Real-time face detection & recognition** using OpenCV's Haar Cascade + LBPH (Local Binary Patterns Histogram) algorithm
- **Confirmation delay** — a face must be visible for a configurable number of seconds before attendance is marked, reducing false positives from a quick glance or someone walking past
- **Automatic Excel logging** — attendance is written to `attendance.xlsx`, with a new dated column added each session
- **Color-coded spreadsheet** — Present rows are highlighted green, Absent rows red, with styled headers and borders (via `openpyxl`)
- **Live on-screen dashboard (HUD)** showing date, total/present/absent counts, and attendance percentage
- **Multi-photo support per person** — add multiple reference photos (e.g. `Ravi_1.jpg`, `Ravi_2.jpg`) to improve recognition accuracy
- **Zero heavy dependencies** — no GPU, no deep learning frameworks; runs on a standard laptop webcam

---

## 📸 Demo

> *(Add a screenshot or short GIF of the webcam window + the generated Excel sheet here)*

| Webcam View | Excel Output |
|---|---|
| `screenshot-webcam.png` | `screenshot-excel.png` |

---

## 🛠️ Tech Stack

| Component | Purpose |
|---|---|
| `opencv-python` | Face detection (Haar Cascade) & recognition (LBPH) |
| `pandas` | Reading/writing attendance data |
| `openpyxl` | Excel styling (colors, fonts, borders, column widths) |
| `numpy` | Array handling for face data |

---

## 📂 Project Structure

```
attendance-system/
│
├── attendance.py          # Main application
├── known_faces/           # Reference photos of each person
│   ├── Ravi.jpg
│   ├── Priya_1.jpg
│   ├── Priya_2.jpg
│   └── ...
├── attendance.xlsx         # Auto-generated attendance log (created on first run)
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/face-recognition-attendance-system.git
cd face-recognition-attendance-system
```

### 2. Install dependencies
```bash
pip install opencv-python pandas openpyxl numpy
```

> **Note:** This project uses `cv2.face`, which comes from the `opencv-contrib-python` package. If `cv2.face.LBPHFaceRecognizer_create()` raises an `AttributeError`, install the contrib package instead:
> ```bash
> pip uninstall opencv-python
> pip install opencv-contrib-python
> ```

### 3. Add reference photos
Create a `known_faces/` folder (auto-created on first run) and add one or more clear, front-facing photos per person:
```
known_faces/
├── Ravi.jpg
├── Priya_1.jpg
├── Priya_2.jpg
```
> Multiple photos of the same person should share a name prefix before an underscore (e.g. `Priya_1.jpg`, `Priya_2.jpg`) — the script groups them automatically.

### 4. Run the app
```bash
python attendance.py
```

- A webcam window will open and start detecting faces.
- When a known face is detected continuously for **2 seconds** (configurable), it's marked **Present** and saved to `attendance.xlsx`.
- Press **`Q`** to quit and save the session.

---

## ⚙️ Configuration

All key settings are at the top of `attendance.py`:

```python
KNOWN_FACES_DIR       = "known_faces"   # Folder with reference photos
EXCEL_FILE            = "attendance.xlsx"  # Output spreadsheet
CONFIRM_SECONDS       = 2               # Seconds of continuous detection before marking present
CONFIDENCE_THRESHOLD  = 80              # Lower = stricter matching (0-100)
```

---

## 🧠 How It Works

1. **Training:** On startup, the script loads every photo in `known_faces/`, detects the face in each using a Haar Cascade classifier, and trains an LBPH recognizer on the cropped, grayscale face regions.
2. **Detection loop:** Each webcam frame is converted to grayscale and scanned for faces. Detected faces are compared against the trained model, returning a predicted identity and a distance score (converted to a confidence percentage).
3. **Confirmation window:** A recognized face must stay in view for `CONFIRM_SECONDS` before being logged — this prevents accidental misfires from a passing glance.
4. **Logging:** Once confirmed, the person's status for today's date is updated to `"Present"` in a Pandas DataFrame, which is saved to Excel and re-styled (colors, borders, column widths) on every write.
5. **Session summary:** On exit, a final report prints total present/absent counts and the attendance percentage to the console.

---

## 🔭 Possible Improvements

- Swap LBPH for a deep-learning embedding model (e.g. `face_recognition` / FaceNet) for higher accuracy at scale
- Add a simple GUI (Tkinter/PyQt) instead of the console + OpenCV window combo
- Support multiple camera sources or IP cameras
- Export weekly/monthly summary reports
- Add liveness detection to prevent spoofing via photos

---

## ⚠️ Limitations

- LBPH is a classical (non-deep-learning) recognition method — accuracy is lower than modern embedding-based approaches, especially with poor lighting, side angles, or many people in the dataset
- Performance depends heavily on the quality of reference photos in `known_faces/`
- No anti-spoofing — a printed photo could fool the recognizer

---

## 📄 License

This project is licensed under the MIT License — feel free to use, modify, and distribute.

---


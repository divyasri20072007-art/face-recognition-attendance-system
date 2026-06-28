# 🟢 Smart Attendance System (OpenCV + LBPH)

An automated, camera-based attendance solution that recognizes people in real time and logs their presence straight into an Excel file — no manual roll call needed. Built entirely on **OpenCV**, with no `dlib`, `deepface`, or `TensorFlow` dependencies.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![OpenCV](https://img.shields.io/badge/OpenCV-LBPH-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## ✨ What it does

- Detects faces from a live webcam feed using a **Haar Cascade classifier**
- Identifies people using OpenCV's **LBPH (Local Binary Patterns Histogram)** recognizer
- Waits for a short, configurable **confirmation window** before marking someone present — so a quick glance or a person walking by doesn't trigger a false mark
- Writes attendance straight into an **Excel spreadsheet**, automatically adding a new column for each day
- Formats the spreadsheet automatically — **green for Present, red for Absent**, styled headers, clean column widths
- Displays a **live heads-up display** on screen showing the date, total/present/absent counts, and attendance percentage
- Supports **multiple training photos per person** for better accuracy (e.g. `Arjun_1.jpg`, `Arjun_2.jpg`)
- Lightweight enough to run on a regular laptop webcam — no GPU required

---

## 📸 Preview

> *(Drop a screenshot or short clip of the live webcam window and the generated spreadsheet here)*

| Live Detection | Excel Log |
|---|---|
| `demo-webcam.png` | `demo-excel.png` |

---

## 🧰 Built With

| Library | Role |
|---|---|
| `opencv-python` (contrib) | Face detection & LBPH recognition |
| `pandas` | Managing attendance data |
| `openpyxl` | Styling the Excel output |
| `numpy` | Handling face image arrays |

---

## 📁 Folder Layout

```
smart-attendance-opencv/
│
├── attendance.py        # Main script
├── known_faces/         # Reference photos, one or more per person
│   ├── Arjun.jpg
│   ├── Neha_1.jpg
│   ├── Neha_2.jpg
│   └── ...
├── attendance.xlsx       # Generated automatically on first run
└── README.md
```

---

## ⚡ Setup & Usage

### 1. Clone this repo
```bash
git clone https://github.com/<your-username>/smart-attendance-opencv.git
cd smart-attendance-opencv
```

### 2. Install requirements
```bash
pip install opencv-contrib-python pandas openpyxl numpy
```

> The `cv2.face` module (needed for LBPH) ships with `opencv-contrib-python`, not the base `opencv-python` package. If you hit an `AttributeError` on `cv2.face`, this is why — install the contrib version instead.

### 3. Add face photos
Put one or more clear, front-facing photos of each person inside `known_faces/`:
```
known_faces/
├── Arjun.jpg
├── Neha_1.jpg
├── Neha_2.jpg
```
> Photos sharing a name prefix before an underscore (like `Neha_1.jpg` / `Neha_2.jpg`) are automatically grouped as the same person, improving recognition accuracy.

### 4. Run it
```bash
python attendance.py
```
- The webcam opens and starts scanning for faces.
- Hold steady in frame for a couple of seconds to get marked **Present**.
- Press **`Q`** any time to exit — the spreadsheet saves automatically.

---

## 🔧 Settings

Adjustable constants at the top of `attendance.py`:

```python
KNOWN_FACES_DIR       = "known_faces"     # Folder of reference photos
EXCEL_FILE            = "attendance.xlsx"    # Output file
CONFIRM_SECONDS       = 2                 # Seconds needed to confirm presence
CONFIDENCE_THRESHOLD  = 80                # Lower = stricter matching
```

---

## 🧩 Under the Hood

1. **Setup phase** — every photo in `known_faces/` is loaded, faces are cropped out using a Haar Cascade, and an LBPH model is trained on the cropped grayscale faces.
2. **Live recognition** — each webcam frame is scanned for faces; detected faces are compared against the trained model to get a predicted name and a confidence score.
3. **Confirmation logic** — a person must remain recognized continuously for a few seconds before being logged, avoiding accidental false positives.
4. **Excel logging** — confirmed attendance updates a Pandas DataFrame, which is saved and re-styled (colors/borders/widths) on every update.
5. **Wrap-up** — when you quit, a summary of who attended and the overall attendance percentage prints to the console.

---

## 🚧 Future Improvements

- Replace LBPH with a deep-learning-based recognizer (e.g. `face_recognition`, FaceNet) for better accuracy
- Build a simple GUI instead of console + OpenCV window
- Add support for IP cameras / multiple camera feeds
- Generate weekly/monthly attendance summaries
- Add basic anti-spoofing (liveness check) so photos can't fool the camera

---

## ⚠️ Known Limitations

- LBPH is a classical algorithm — less accurate than modern deep-learning face recognition, especially with poor lighting or many faces in the database
- Recognition quality depends heavily on how good the reference photos are
- No protection against someone holding up a photo instead of their face

---

## 📄 License

Released under the MIT License — free to use, modify, and share.

---

## 🙋 Author

Made by **[Friend's Name]**

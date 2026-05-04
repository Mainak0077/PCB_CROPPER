# 🟢 PCB Cropper — Automated Board Segmentation Tool

> Automatically detects and crops individual PCB boards from a single grid image. Built with Python, Flask, and OpenCV.

![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat-square&logo=python)
![Flask](https://img.shields.io/badge/Flask-3.1-black?style=flat-square&logo=flask)
![OpenCV](https://img.shields.io/badge/OpenCV-4.13-green?style=flat-square&logo=opencv)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

---

## 📸 What It Does

Got a single image containing a grid of PCB boards (like 2×5, 3×4, etc.)? PCB Cropper automatically:

1. Detects each individual board using computer vision
2. Crops them precisely with configurable padding
3. Names them sequentially with a custom start counter (e.g. `pcb_55.png`, `pcb_56.png`...)
4. Packages everything into a ZIP file for one-click download

No manual cropping. No renaming. Just upload and download.

---

## ⚙️ How It Works

```
Input Image
    │
    ▼
Grayscale + Gaussian Blur
    │
    ▼
Otsu Auto-Threshold (binarize)
    │
    ▼
Morphological Close (fill PCB component gaps)
    │
    ▼
Contour Detection → Filter by area + aspect ratio
    │
    ├─ Count matches? → Use contour boxes
    │
    └─ Count mismatch / too few? → Grid Fallback
          (auto-factor into best grid layout)
    │
    ▼
Sort boxes: top→bottom, left→right
    │
    ▼
Crop each board + padding → save as pcb_N.png
    │
    ▼
Annotated preview + ZIP download
```

---

## 🚀 Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/pcb-cropper.git
cd pcb-cropper
```

### 2. Create a virtual environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the app

```bash
python app.py
```

### 5. Open in browser

```
http://localhost:5000
```

> ⚠️ **Important:** Always access via `http://localhost:5000` — do NOT open `index.html` directly as a file. Direct `file://` access blocks the Flask API calls.

---

## 📁 Project Structure

```
pcb-cropper/
├── app.py                  ← Flask backend + OpenCV detection logic
├── requirements.txt        ← Python dependencies
├── README.md
├── templates/
│   └── index.html          ← Frontend UI (served by Flask)
└── output/                 ← Auto-created; session crops saved here
    └── <session_id>/
        ├── pcb_55.png
        ├── pcb_56.png
        ├── ...
        ├── preview.jpg     ← Annotated detection overlay
        └── pcb_crops.zip   ← Downloadable bundle
```

---

## 🎛️ Features

| Feature | Details |
|---|---|
| **Auto-detection** | Otsu threshold + contour filtering |
| **Grid fallback** | Kicks in if detection fails; tries common layouts (2×5, 3×4, etc.) |
| **Custom start counter** | Name files from any number (e.g. 55, 100) |
| **Annotated preview** | See detected regions before downloading |
| **Per-card download** | Download individual crops |
| **ZIP download** | All crops bundled in one click |
| **Formats supported** | PNG, JPG, BMP, TIFF, WEBP |
| **Max upload size** | 50 MB |

---

## 🧪 Usage Tips

- **PCBs on white/light background** give the best detection results
- Use the **PCB Count** hint field if auto-detection gives the wrong count
- Use **Start Counter From** to continue sequential naming across batches (first batch → 1, next → 11, next → 21, etc.)
- If detection misses boards, specify count → grid fallback will divide the image evenly

---

## 📦 Requirements

```
flask>=3.0.0
opencv-python-headless>=4.9.0
numpy>=1.26.0
werkzeug>=3.0.0
```

> **Note:** If you have existing projects using older numpy (langchain, streamlit, tensorflow), use a virtual environment to avoid conflicts.

---

## 🔧 Configuration

Edit the top of `app.py` to adjust defaults:

| Constant | Default | Purpose |
|---|---|---|
| `MAX_CONTENT_LENGTH` | 50 MB | Max upload size |
| `min_area` | 0.5% of image | Minimum PCB contour area |
| `max_area` | 60% of image | Maximum PCB contour area |
| `padding` in `refine_crop` | 8 px | Padding around each crop |

---

## 🛠️ Tech Stack

- **Backend** — Python 3.11, Flask 3.x
- **Image Processing** — OpenCV 4.x, NumPy
- **Frontend** — Vanilla HTML/CSS/JS (no build step)
- **Font** — IBM Plex Mono + IBM Plex Sans

---

## 👤 Author

**Mainak Karmakar**  
MCA Student @ HIT Kolkata  
[GitHub](https://github.com/yourusername) · [LinkedIn](https://linkedin.com/in/yourprofile)

---

## 📄 License

MIT License — free to use, modify, and distribute.

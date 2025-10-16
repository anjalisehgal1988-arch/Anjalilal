# Gaze Detection Annotation Tool

Lightweight Flask app to label gaze annotations on images. Draw a face bounding box, click the gaze target, and submit — annotations are saved into `annotations.json` with automatic indexing.

## Features
- Label multiple persons per image with gaze lines and numbered labels.
- Simple form fields: target type, distance estimate, and object category.
- Serves a fixed set of up to 500 images from `merged_images` so all users see the same dataset.
- Image paths resolved from `merged_images/gazefollow` and `merged_images/vat`.

## Requirements
- Python 3.9+
- Pip (`pip --version`)

## Setup
- (Optional) create a virtual environment:
  - Windows: `python -m venv .venv && .\.venv\Scripts\activate`
  - macOS/Linux: `python3 -m venv .venv && source .venv/bin/activate`
- Install dependencies: `pip install -r requirements.txt`
- Ensure data exists:
  - `combined_gazefollow_vat.json` in repo root.
  - Images copied to `merged_images/gazefollow/...` and `merged_images/vat/...`.

## Run
- Default port: `python app.py`
- Custom port (Windows): `$env:PORT=5001; python app.py`
- Custom port (macOS/Linux): `PORT=5001 python app.py`
- Open: `http://127.0.0.1:5000/` (or your chosen port)

### One-click (Windows)
- Double-click `run.bat`.
- Optional: pass a port as the first argument, e.g., `run.bat 5001`.

## Usage
- The app redirects to the first image and renders the labeling UI.
- Draw rectangle → click gaze point → repeat as needed.
- Use “Reset Last Annotation” to undo the most recent entry.
- Click “Submit All” to save for the current image and advance to the next.
- Saved to `annotations.json` as:
  - `[{ "index": <auto_id>, "annotations": [{ bbox, gaze, gaze_number, ... }] }, ... ]`

## Data Notes
- This repository ignores heavy datasets by default via `.gitignore`:
  - `Gazefollow/`, `VAT/images/`, `merged_images/`, and `labels.csv` are excluded.
  - If you need a few demo images in Git, place them under `samples/` and adjust `.gitignore` accordingly.

## GitHub
- Suggested repo: `https://github.com/anjalisehgal1988-arch/gaze_detection`
- Initial push:
  - `git init && git branch -M main`
  - `git add . && git commit -m "Initial commit"`
  - `git remote add origin https://github.com/anjalisehgal1988-arch/gaze_detection.git`
  - `git push -u origin main`

### If origin already points to Thesis
- Update remote URL:
  - `git remote set-url origin https://github.com/anjalisehgal1988-arch/gaze_detection.git`

## Environment Variables
- `PORT`: HTTP port to bind (default `5000`).
- `ANNOTATIONS_PATH`: where to save annotations (default `./annotations.json`).
  - In production, set to a persistent disk path, e.g., `/var/data/annotations.json`.
- `MERGED_ROOT`: base folder for images (default `./merged_images`).
  - In production, point to a mounted volume, e.g., `/var/data/merged_images`.

## Deployment (Render)
- This repository includes `render.yaml` for one-click deployment.
- Steps:
  - Push to GitHub.
  - On Render: New → Blueprint → select this repo.
  - Render will provision a Web Service with:
    - Build: `pip install -r requirements.txt`
    - Start: `gunicorn app:app --bind 0.0.0.0:$PORT --workers 2`
    - Disk mounted at `/var/data`.
    - Env vars:
      - `ANNOTATIONS_PATH=/var/data/annotations.json`
      - `MERGED_ROOT=/var/data/merged_images`
  - Upload your images to the mounted disk (`/var/data/merged_images`).
  - Open the public URL; annotations persist to `/var/data/annotations.json`.

## License
- See `LICENSE` for terms.
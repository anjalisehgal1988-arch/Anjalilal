Quickstart: Local Annotation

Requirements
- Python 3.9+ installed
- Internet not required once kit is unzipped

Windows
- Double-click run.bat
- Optional: personalize the output file
  - Open Command Prompt in the project folder
  - Run: set ANNOTATIONS_PATH=%CD%\annotations_<YOUR_ID>.json
  - Then: python app.py

macOS/Linux
- In terminal, run:
  - python3 -m venv .venv && source .venv/bin/activate
  - pip install -r requirements.txt
  - Optional personalized path: ANNOTATIONS_PATH=$PWD/annotations_<YOUR_ID>.json python3 app.py
  - Otherwise: python3 app.py

Annotate
- Open http://127.0.0.1:5000/
- Draw face box, click gaze target, fill labels, click “Submit All”
- Repeat for the images shown

Return Results
- Find the annotations file in the project folder
  - If you set ANNOTATIONS_PATH, send that file (annotations_<YOUR_ID>.json)
  - Otherwise, rename annotations.json to annotations_<YOUR_ID>.json and send it
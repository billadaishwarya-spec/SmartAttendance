# VisiClass Phase 1 — Face Recognition Attendance from a Recorded Video Feed

Phase 1 of a 4-part system that processes a local video file (simulating CCTV), recognizes student faces against an image directory, and logs attendance with timestamps to a live Flask web dashboard.

---

## Project Structure

```text
SmartAttendance/
  requirements.txt
  train_recognizer.py       # Step 1: learn faces from known_students/
  process_video.py          # Step 2: process video and mark attendance
  app.py                    # Step 3: Flask web dashboard
  known_students/           # Directory for student reference photos
  templates/index.html      # Dashboard template
  static/style.css          # CCTV monitor theme styling
  static/app.js             # Live-polling dashboard script
  data/
    haarcascade_frontalface_default.xml   # Bundled face detector
    lbph_model.yml                         # Generated trained model
    labels.txt                             # Generated label map
    attendance_status.json                 # Generated live attendance log
```

---

## Setup & Execution Guide

1. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
2. **Add Student Photos:** Place 1–3 front-facing photos per student inside `known_students/` (e.g., `known_students/Aishwarya.jpg` or nested folders for multiple angles).
3. **Train Recognizer:** Run `python train_recognizer.py` to generate the model and verify face detection across your student images.
4. **Prepare Video:** Save your classroom footage as an accessible file (e.g., `classroom_video.mp4`).
5. **Process Video:** Run `python process_video.py --video classroom_video.mp4 --skip-frames 4` (add `--display` to view real-time detection).
6. **Launch Dashboard:** Run `python app.py` and open `http://localhost:5000` to view real-time attendance tracking (auto-refreshes every 2 seconds).

---

## Demo Flow & Tips

- **Live Presentation:** Open the dashboard before your session starts (showing students as "Absent"), then execute `process_video.py` in a terminal to watch statuses flip to "Present" with video-time timestamps.
- **Filming Advice:** Use landscape mode, convert `.mov` files to `.mp4` via `ffmpeg`, and prioritize well-lit, close-up frames over wide-angle classroom shots.
- **Tuning & Limits:** Adjust `CONFIDENCE_THRESHOLD` or `--skip-frames` in `process_video.py` to balance processing speed against recognition accuracy. Note that LBPH works best with close-up shots and records attendance upon the first confident detection.

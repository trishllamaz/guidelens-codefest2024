# GuideLens

A real-time narrator web app that helps visually impaired users understand their surroundings. GuideLens watches a live webcam feed, detects objects in it, and announces what it sees and where, like "person, close" or "cup, bottom left", through the browser's built-in speech synthesis.

Built by a team of 5 at **CodeFest 2024**.

## How it works

```
Browser (webcam)  --snapshot every 0.5s-->  Node.js / Express server
                                                   |
                                                   v
                                     Python + MediaPipe object detection
                                       (EfficientDet-Lite, TFLite)
                                                   |
Browser speaks results  <--detected objects + positions--+
   (Web Speech API)
```

1. **Capture.** The front end grabs a frame from the webcam every 0.5 seconds and POSTs it to the server as a JPEG.
2. **Detect.** The Express server saves the frame and runs `classifyImage.py`, which uses Google MediaPipe's object detector with an EfficientDet-Lite model (confidence threshold 0.5).
3. **Locate.** Each detected object's bounding box is mapped to a region of the frame: top or bottom, left, middle or right, or **close** when the object fills most of the view.
4. **Announce.** The browser reads the results back and speaks them aloud with the Web Speech API.

## Tech stack

- **Front end:** HTML, CSS, JavaScript, MediaDevices (webcam) API, Web Speech API
- **Server:** Node.js, Express
- **Computer vision:** Python, Google MediaPipe Tasks, EfficientDet-Lite (TFLite), NumPy, OpenCV

## Running it locally

Requires Node.js and Python 3.

```bash
cd web_webcam

# Node dependencies
npm install

# Python dependencies
pip install mediapipe opencv-python numpy pillow requests

# Start the server
node server.js
```

Open http://localhost:3000, allow camera access, and turn up your volume.

> The server calls `python` to run the detector. If your system uses `python3`, change the command in `server.js`.

## Project structure

```
web_webcam/
├── server.js            # Express server: receives snapshots, runs the detector
├── classifyImage.py     # MediaPipe object detection + position mapping
├── efficientdet.tflite  # EfficientDet-Lite model
└── public/
    ├── index.html       # Webcam capture, polling, and speech output
    └── styles.css
```

## Team

Built at CodeFest 2024 by a team of 5: Andy Chek, Charles Wu, Ricky Chen, Keith Trungcao, and Patricia Tran.

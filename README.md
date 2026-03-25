# RockLook
RockLook 🎸
RockLook is a tiny head‑controlled music trigger: it uses your webcam and MediaPipe Face Mesh to detect when you tilt your head down, and plays or pauses a music track accordingly. It’s built for the BUILDCORED ORCAS — Day 01 challenge.

How it works
Uses OpenCV to read frames from your webcam in real time.
​

Runs MediaPipe Face Mesh to track your iris centers and nose tip on each frame.

Computes a gaze offset (iris_y - nose_y) and compares it to a configurable threshold.

When you look down, it starts or resumes the music; when you look up, it pauses, with edge detection to avoid spamming play/pause.

Requirements
Python 3.10.x

A working webcam

An MP3 file in the project folder named music.mp3

Python packages:

bash
pip install mediapipe==0.10.13 opencv-python pygame
(Adjust the MediaPipe version to one that supports mp.solutions.face_mesh for your Python version.)

Setup
Clone this repository or download the script.

Place any .mp3 file next to the script and rename it to music.mp3.

Ensure your webcam is connected and not used by other apps.

Usage
Run the script with Python:

bash
python rocklook.py
You’ll see a webcam window with:

Live video mirrored like a selfie.

Text overlay showing:

Gaze offset (iris vs nose vertical distance)

Threshold (your chosen trigger point)

Current status: LOOKING DOWN or LOOKING UP

Controls:

Tilt your head down past the threshold → music starts or unpauses.

Tilt your head up above the threshold → music pauses.

Press q → quit and release the camera.

Tuning the gaze threshold
The key parameter is:

python
GAZE_THRESHOLD = -0.13
More negative → you must look further down to trigger.

Closer to 0 → triggers more easily with smaller head movements.

Watch the Gaze offset value in the overlay while looking straight vs. clearly down, then adjust GAZE_THRESHOLD until it feels natural.

Core logic (high level)
Track iris and nose tip Y coordinates.

Compute

python
gaze_offset = iris_y - nose_y
looking_down = gaze_offset < GAZE_THRESHOLD
Detect state changes only:

Transition UP → DOWN:

If music has never played: pygame.mixer.music.play(-1) (loop).

Else: pygame.mixer.music.unpause().

Transition DOWN → UP:

pygame.mixer.music.pause().

A small was_looking_down flag ensures play/pause triggers only once per movement, not every frame.

Credits
Face tracking: MediaPipe Face Mesh.

Video capture: OpenCV.
​

Audio playback: pygame.mixer.music.
​

Built as part of the BUILDCORED ORCAS — Day 01: RockLook challenge.

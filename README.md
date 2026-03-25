# RockLook 🎸

RockLook is a tiny **head‑controlled music trigger**: it uses your webcam and MediaPipe Face Mesh to detect when you tilt your head down, and plays or pauses a music track accordingly. It’s built for the **BUILDCORED ORCAS — Day 01** challenge.

## How it works

- Uses OpenCV to read frames from your webcam in real time.
- Runs MediaPipe Face Mesh to track your **iris centers** and **nose tip** on each frame.
- Computes a **gaze offset** (`iris_y - nose_y`) and compares it to a configurable threshold.
- When you **look down**, it starts or resumes the music; when you **look up**, it pauses, with edge detection to avoid spamming play/pause.

## Requirements

- Python 3.10.x
- A working webcam
- An MP3 file in the project folder named `music.mp3`

## Python packages:

```bash
pip install mediapipe==0.10.13 opencv-python pygame

```
(Use a MediaPipe version that supports mp.solutions.face_mesh for your Python version.)

## Setup

1. Clone this repository or download the script.
2. Place any `music.mp3` file next to the script.
3. Ensure your webcam is connected and not used by other apps.
4. Install the required Python packages (see Requirements section).

## Usage

# From the project folder:
python rocklook.py

- A webcam window opens, mirrored like a selfie.
- On-screen overlay shows:
  - Gaze offset (iris vs nose vertical distance)
  - Threshold (trigger point)
  - Status: LOOKING DOWN or LOOKING UP

## Controls:

- Look DOWN past the threshold  → music starts or unpauses.
- Look UP above the threshold   → music pauses.
- Press `q`                     → quit and release the camera.
Tuning the gaze threshold
python
GAZE_THRESHOLD = -0.13
text
- Make it more negative → you must look further down to trigger.
- Move it closer to 0   → triggers more easily with smaller head movements.

## Tip:

- Watch the "Gaze offset" value:
  - Look straight ahead → note the value.
  - Look clearly down   → note the value.
- Pick a threshold between those that feels natural.
Core logic (high level)
python
# 1. Track iris and nose tip Y coordinates
gaze_offset = iris_y - nose_y

# 2. Decide if looking down
looking_down = gaze_offset < GAZE_THRESHOLD
python
# 3. Edge-based music control
if looking_down and not was_looking_down:
    # UP -> DOWN transition
    if os.path.exists(MUSIC_FILE):
        if not is_playing:
            pygame.mixer.music.play(-1)  # start once, loop
            is_playing = True
            print("▶ Music started")
        else:
            pygame.mixer.music.unpause()
            print("▶ Music unpaused")

elif not looking_down and was_looking_down:
    # DOWN -> UP transition
    if is_playing:
        pygame.mixer.music.pause()
        print("⏸ Music paused")

# 4. Update state
was_looking_down = looking_down
A small was_looking_down flag ensures actions happen only when you cross the threshold, not every frame.

## Credits

- Face tracking: MediaPipe Face Mesh
- Video capture: OpenCV
- Audio playback: pygame.mixer.music

Built as part of the BUILDCORED ORCAS — Day 01: RockLook challenge.

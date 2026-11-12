# blinkcam

Blink to take a picture. The page watches your face through the webcam with MediaPipe Face Landmarker, and every time you blink it snaps the area around your eye and stacks it on the screen as a numbered window. While you keep your eyes open a drone tightens; the moment you blink, a shutter clicks. After 16 shots the windows line up in a grid in order, and it starts over.

- Live: https://verytired.github.io/blinkcam/
- The camera image is used only inside your browser. Nothing is sent or saved.

Cut out from "blink", a piece in the ひとりaudio/visual series that does the same thing with three AI-generated faces.

## Usage

```bash
python3 -m http.server
```

Open `http://localhost:8000/`, press "◉ カメラで始める" and allow the camera. While running, "● 30秒録画" records 30 seconds with sound, and "終わる" (or Esc) stops the camera and returns to the top.

## How it works

- **Blink detection**: the average of the `eyeBlinkLeft` / `eyeBlinkRight` blendshapes. Above 0.45 counts as closed, below 0.25 as open again; a shot is taken on reopening (at least 0.12 s apart).
- **Snapshot**: a 2:1 box 1.9× the eye width, built from the eye contour landmarks. The brighter-lit eye is chosen when the face is first found.
- **Picture**: mirrored, grayscale with high contrast and grain. The longer you hold your eyes open, the more the virtual camera zooms toward your eye.
- **Sound** (Web Audio): detuned sawtooths through a lowpass plus a 3520 Hz ring that rise with the hold time; a shutter click and a pentatonic blip per shot, pitched up with the shot count; ticks and a low chord when the grid forms.

`preview.mp4` is the clip on the top page (11 s from the blink render, 540 px, no audio).

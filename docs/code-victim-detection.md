# Spotting a victim sign

To find a victim sign in the camera image from [the last page](code-camera.md), this page adapts
the official `victim_detection_test.py` sample. It takes about 20 minutes and is the first page in
this series that touches something the rules actually score. The most useful thing on it is a
number we had to change from the official sample to get it working at all.

!!! note "Credit where it's due"
    The detection function on this page is the official Erebus `victim_detection_test.py` sample,
    lightly adapted. We changed one number and explain exactly why below.

---

## Step 1: What a victim sign looks like to code

A victim sign is a small, high-contrast picture (a stick figure, in the run we tested) painted on a
wall. The official approach: convert the camera image to greyscale, threshold it so anything darker
than a cutoff turns solid black, then ask OpenCV for the outlines (**contours**) of the dark shapes
in what's left. A big enough dark contour is probably a sign.

```python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
thresh = cv2.threshold(gray, 140, 255, cv2.THRESH_BINARY)[1]
contours, h = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
```

"Big enough" is a number you have to tune, `cv2.contourArea(c)`, in pixels. The official sample
uses `1000`. Step 3 is about why that number didn't work for us.

---

## Step 2: Write the controller

```python
from controller import Robot
import cv2
import numpy as np


def detectVisualSimple(image_data, camera):
	coords_list = []
	img = np.array(np.frombuffer(image_data, np.uint8).reshape((camera.getHeight(), camera.getWidth(), 4)))
	img[:,:,2] = np.zeros([img.shape[0], img.shape[1]])

	gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
	thresh = cv2.threshold(gray, 140, 255, cv2.THRESH_BINARY)[1]

	contours, h = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

	for c in contours:
		if cv2.contourArea(c) > 150:
			coords = list(c[0][0])
			coords_list.append(coords)
			print("Victim at x="+str(coords[0])+" y="+str(coords[1])+" area="+str(cv2.contourArea(c)))

	return coords_list


robot = Robot()
timeStep = 32

camera = robot.getDevice("camera_centre")
camera.enable(timeStep)

while robot.step(timeStep) != -1:
	img = camera.getImage()
	detectVisualSimple(img, camera)
```

The only change from the official sample is `150` where it originally said `1000`, and printing
`area` alongside the coordinates so we could see why.

---

## Step 3: Why the official threshold didn't work here

To see what the camera was actually reporting, we pointed the robot straight at a real victim
sign from about 15 cm away, close enough that the sign filled a real portion of the frame, and
printed every contour's area, even small ones.

!!! success "You should now see"
    With the original `1000` threshold: nothing. Not one detection, even standing right in front of
    a sign. The largest contour area we ever measured, at the closest distance we tried, was:

    ```
    max contour area this frame: 215.0
    ```

    `camera_centre` on this build is only 64×40 pixels (confirmed in
    [Turning on the camera](code-camera.md)), 2560 pixels total. A contour of `1000` pixels would
    need to cover **39% of the entire image**. At this resolution, a sign essentially never gets
    that big before the robot is uncomfortably close to the wall. The official sample's threshold
    looks like it was tuned for a higher-resolution camera than this robot's default.

    Lowering the threshold to `150` immediately started working:

    ```
    Victim at x=2 y=10 area=212.0
    Victim at x=5 y=10 area=192.5
    Victim at x=7 y=10 area=190.5
    Victim at x=9 y=10 area=201.5
    Victim at x=12 y=10 area=184.5
    ```

    The `x` coordinate climbs steadily frame to frame as the robot rotates past the sign, real
    tracking, not noise, and the decoded camera frame from the same moment confirms it:

    ![The real victim sign as the robot's camera saw it, contour area 212](assets/real/c6-victim-view.png)

    *Decoded directly from `camera.getImage()`, the same technique as the last page. This is the
    real sign that produced the `area=212.0` line above.*

### How we actually got the robot in front of a sign

Reliable navigation to a specific spot in the maze is still an open problem on this site.
[The wall-follower page](code-wall-follower.md) found that even a dedicated wall-following
controller settles into a small loop rather than touring world1. Therefore, for this page, we used
our own trial harness's supervisor access to place the robot a known short distance from a known
victim sign, the same way a team might manually position a robot on their desk to test detection
code in isolation before trusting it to navigate there on its own. That positioning step is not
something your controller can do, and it isn't shown above. The code above is exactly what ran and
produced the output on this page.

### What about false positives?

We ran this same `150`-threshold detector for 90 seconds while the robot explored on its own with
[the wall-follower from two pages ago](code-wall-follower.md), away from any known victim, and
**didn't get a single false detection** in that test. However, that is a real result, not a
guarantee: `150` is a much lower bar than the official `1000`, and a lower bar is inherently
more likely to mistake a dark shadow or wall corner for a sign somewhere we haven't tried yet. Treat `150` as a starting
point you should re-check in every world you actually compete in, not a number to trust blindly.

---

## Now make it your own

- Try threshold values between `150` and `1000` and see roughly where detection stops working at
  the distance this page used.
- Additionally, print `len(contours)` as well as the accepted ones. On a busy frame there are often several small
  contours that never cross the threshold, and that's normal, not a bug.
- Combine this with [driving](code-driving.md): once you detect a victim, what should the robot do
  next? The next page in this series answers that.

---

## If it goes wrong

- **Nothing is ever detected, no matter how close you get.** Double check your threshold isn't still
  `1000`, this page exists because that number doesn't work at this camera's resolution.
- **`img[:,:,2] = np.zeros(...)` raises a shape error.** `camera.getHeight()` and `.getWidth()` must
  match the image you actually got. If you changed the camera's resolution, re-check both.
- **You get detections, but the coordinates don't move smoothly frame to frame like this page's
  example.** That's a sign you might be picking up noise rather than a real sign, try raising the
  threshold a little and see if the jumpy detections disappear while the real ones stay.

---

Next: reporting a victim and earning your first points (coming soon).

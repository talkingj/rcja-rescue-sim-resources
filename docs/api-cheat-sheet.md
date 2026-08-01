# Erebus API cheat sheet

Every device name, method, and message byte format this site actually confirmed while building
[Track C](code-sensors.md), in one place. Therefore, every entry cites the resource where it was
verified, and nothing here comes from documentation alone — if it's listed, this site ran it for
real.

!!! note "Where this fits"
    A reference page, not a tutorial. If a row here doesn't make sense on its own, follow its link
    to the page that teaches it properly.

## Devices

| Device name | What it is | `.enable()` needed? | Confirmed in |
|---|---|---|---|
| `"wheel1 motor"` / `"wheel2 motor"` | Wheel motors. `.setPosition(float("inf"))` once, then `.setVelocity(<rad/s>)` | No | [C1](code-sensors.md) |
| `"ps0"`-`"ps7"` | Eight distance sensors around the e-puck ring | Yes | [C1](code-sensors.md) |
| `"wheel1 sensor"` / `"wheel2 sensor"` | Wheel position sensors (encoders). `.getValue()` returns total radians since start, never resets itself | Yes | [C2](code-driving.md) |
| `"colour_sensor"` | The floor colour sensor. It's a **Camera** device, 1×1 resolution, not a plain analogue sensor | Yes | [C3](code-colour.md) |
| `"camera_centre"` | Main forward camera. `64×40` resolution, `10240`-byte BGRA buffer, `fov≈1.0000` rad on this build | Yes (for `.getImage()`) | [C5](code-camera.md) |
| `"emitter"` | The robot's only outbound channel to the supervisor | No | [C7](code-reporting.md) |
| `"receiver"` | The robot's only inbound channel from the supervisor | Yes | [C7](code-reporting.md) |

`camera_left` and `camera_right` also exist on the robot's proto (untested this session, same BGRA
format expected, [confirmed present in C5](code-camera.md)).

## Confirmed physical constants (this robot, this build)

| Constant | Value | Confirmed in |
|---|---|---|
| Wheel radius | `0.02` m | [C2](code-driving.md), read from `custom_robot.proto` |
| Wheel track width | `0.052` m | [C2](code-driving.md), read from `custom_robot.proto` |
| Distance sensor scale | low = close (~`0.01`), high = clear (~`0.8`) | [C1](code-sensors.md) |
| A measured 90° turn | `2.28`-`2.30` rad of wheel rotation | [C2](code-driving.md), confirmed via four consecutive turns |
| `timeStep` used throughout | `32` ms | Every Track C resource |
| Identification radius | fixed `0.09` m, both real and estimated position | [C7](code-reporting.md), read from `Victim.py:check_position` |
| Game clock length | ~480s (8 min) | [C8](code-game-info.md) |
| Real-world clock length | ~600s (10 min) | [C8](code-game-info.md) |
| LoP passive timeout | 20s of measured stillness | [C9](code-lack-of-progress.md) |
| Exit bonus | `+10%` of current score, two conditions | [C10](code-exit.md) |
| Map bonus formula | `score × correctness × 1.2`, added at exit | [C11](code-mapping.md) |

## Message formats (all via `emitter`/`receiver`)

| Message | Bytes | Format string | Confirmed in |
|---|---|---|---|
| Game info request | 1 | `struct.pack('c', b'G')` | [C8](code-game-info.md) |
| Game info reply | 16 | `struct.pack("c f i i", b'G', score, game_time_left, real_time_left)` | [C8](code-game-info.md) |
| Victim/hazmat report | 9 | `struct.pack('i i c', x_cm, z_cm, type_char)`, coordinates in **whole centimetres** | [C7](code-reporting.md) |
| LoP request | 1 | `struct.pack('c', b'L')` | [C9](code-lack-of-progress.md) |
| LoP acknowledgement (received) | 1 | Same format, `b'L'` echoed back | [C9](code-lack-of-progress.md) |
| Exit | 1 | `struct.pack('c', b'E')` | [C10](code-exit.md) |
| Map data | shape (8) + data | `struct.pack('2i', rows, cols)` then every cell joined with `,`, UTF-8 encoded | [C11](code-mapping.md) |
| Map evaluate request | 1 | `struct.pack('c', b'M')` | [C11](code-mapping.md) |

Victim type letters, confirmed from `Victim.py`: `'H'` = harmed, `'U'` = unharmed, `'S'` = stable.

## Behaviour worth knowing, not obvious from the API alone

- A 9-byte report is silently dropped unless the robot has been stationary for a full second
  (`time_stopped() >= 1.0`), and in practice needs more margin than that right after a spawn or
  relocate to register reliably, [confirmed on C10](code-exit.md#getting-this-run-clean-took-more-patience-than-it-looked-like-it-should).
- A score update isn't guaranteed visible on the very next game-info poll after the action that
  caused it, [confirmed on C8](code-game-info.md).
- `cv2.findContours` on this OpenCV build returns a 2-tuple `(contours, hierarchy)`, not the 3-tuple
  some older documentation shows, [confirmed on C6](code-victim-detection.md).
- Misidentification and LoP penalties are both flat `-5` and do **not** scale with the area
  multiplier, unlike TI/TT/CN, [confirmed on C7/C9](code-reporting.md).
- The map bonus is applied unconditionally at exit, regardless of the exit bonus's own start-tile
  and identification conditions, [confirmed on C11](code-mapping.md).
- Differential-drive dead reckoning (`dcenter = radius×(dl+dr)/2`, `dtheta = radius×(dr-dl)/track`)
  works, but real drift of ~12cm over just 5 seconds of movement was measured on this robot,
  [confirmed on C12](code-complete-run.md).

---

See also: [the debugging playbook](debugging-playbook.md) and [when it goes
wrong](troubleshooting.md) for symptom-indexed fixes, and [the glossary](glossary.md) for any term
here that isn't self-explanatory.

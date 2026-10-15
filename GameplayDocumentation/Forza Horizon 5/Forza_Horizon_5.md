# Forza Horizon 5

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

The integration also carries aspect-ratio fallbacks, so other resolutions sharing those aspect ratios are read from the nearest matching profile.

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the Display / Resolution option, then set the game to the resolution being tested. If it is not available, adjust the Aspect Ratio / Display Mode setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

**Keep the HUD on its default layout.** The Speed meter reads the speedometer digits in the lower-right corner, so a moved, hidden or rescaled speedometer breaks it. The speedometer should show a number (not only the gauge), and the displayed unit must match the **Speed in miles** setting (MPH or KM/H).

---

## Effect Controls

Each effect has its own on/off toggle in the SignalRGB UI: **Enable speed effects**, **Enable go effect**, **Enable fast travel effect**, **Enable finish effect** and **Enable wheelspin effects**. The remaining controls are:

* **Speed in miles** — switches the speed thresholds between MPH and KM/H. It must match the unit shown by the in-game speedometer.
* **High Speed Effect Color** — the color of the high-speed stripes.
* **Ambiance brightness** — the brightness of the screen-ambience backdrop that plays behind the effects (the screen is mirrored onto the devices whenever no effect is covering it).

---

## A Note on Testing Forza Horizon 5

Forza is a driving game, so most meters are triggered by *how you drive*, and a couple of them need a race to be completed:

* **Free roam** — enough to test Speed, High Speed and Fast Travel on demand.
* **A race event** — needed for the ¡GO! start, the Finish screen. Pick a short event (a Circuit or Sprint of a few minutes works well). Any finishing position triggers the Finish effect, so you do not need to win.
* **Wheelspin** — only appears when you have earned one (for example after a level-up) and open it from the reward screen.

**Meters work in pairs**: the Go, Fast Travel, Finish and Wheelspin effects each need *both* of their meters to read high at the same time. A frame that triggers only one of them will not fire the effect.

---

## Testing Guidelines

### 1. Drive — speed effects

Accelerate and brake in free roam. The `SpeedMeter` reads the speedometer number with OCR (`ocr_numeric`).

* **Accelerating** — when the speed climbs more than **5** above the last reference value, green arrows sweep **upward**.
* **Braking** — when the speed drops more than **5** below the last reference value, red arrows sweep **downward**.

Only one arrow animation plays at a time. It only reacts to a *change*, so hold a steady speed and then change it by a clear amount.

![speed](images/speed.png)

### 2. High speed

Keep accelerating past the high-speed threshold: **185 MPH** (or **300 KM/H** with *Speed in miles* off). Stripes in the **High Speed Effect Color** fall across the devices. The effect stops after the speed stays below the threshold for about **3 seconds**.

Speed is also taken from the same `SpeedMeter` as step 1, so no extra screen is needed.

### 3. Fast travel

Open the map and fast travel to any destination. During the loading cinematic, the letterboxed bars and the **FAST TRAVELING TO DESTINATION** text at the bottom right trigger `FastTravelBlack` (the black bar, low lightness) and `FastTravelWhite` (the white text, high lightness) together. Blue bars sweep across the devices, and the animation repeats until the loading screen ends.

Fast travel also clears every other running effect and resets the speed reference.

![fast travel](images/fast_travel.png)

### 4. Race start — ¡GO!

Start a race event and stay on the grid. When the countdown ends and the white **¡GO!** text appears, `Go1` and `Go2` (the two white sides of the text, near-zero saturation and very high lightness) trigger together. The devices flash **green** for 3 seconds. While it plays, the ambience backdrop dims.

![go](images/go.png)

### 5. Finish

Finish a race, in any position. The pink **FINISHED** results screen is read by `Finish1` and `Finish2` (the pink banner tones above and below the title). When both read high, a black wipe covers the devices, and yellow particles then fall like confetti.

Finish also cancels a running High Speed effect.

![finish](images/finish.png)

### 6. Wheelspin

Open a **Horizon Wheelspin** from the reward screen. The pink bands at the top of the wheelspin screen trigger `Wheelspin` and `Wheelspin2` (both must read above 0.9). A cyan and magenta wheel rises from the bottom, spins up, and slows down. It plays for about **7 seconds**.

The effect fires once per screen. Leave the wheelspin screen and enter it again to retrigger it.

![wheelspin](images/wheelspin.png)

---

## Notes

* **Paired meters** — `FastTravelWhite` + `FastTravelBlack`, `Go1` + `Go2`, `Finish1` + `Finish2` and `Wheelspin` + `Wheelspin2` must read together. If one effect never fires, check that both meters are matching.
* **Speed uses the screen number** — the speed effects depend on OCR, so very stylized speedometer layouts, a very low resolution or extreme HUD scaling can reduce accuracy.
* **Go and Finish are not repeated by the same frame** — hold on the screen until the animation ends before starting another test.
* **Finish and Fast Travel have priority** — they clear the lower-priority effects (such as High Speed) while they play.
* **Screenshot frames in this folder** — the QA screenshots in `ForzaHorizon5/` are organized per resolution (`SDR` or `HDR`), then per meter name, and the number at the end of each file name is the match threshold used by the meter (for example `Wheelspin_gt.9` = greater than 0.9, `Go1_gte.95` = at least 0.95).
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

# Aniimo

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

SDR (all resolutions) and HDR (3840x2160)

---

## Resolution Setup

Change the monitor resolution, launch the game and open the display/graphics settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display mode so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Testing Guidelines

The in-game gate reads a stable white HUD element (`inGameWhite1` + `inGameWhite2`, guarded by `ingameNotWhite`) **or** a visible health bar (`healthGreen1`/`healthGreen2` + `healthWhite`). Most effects require the gate; **Battle Won**, **New Story** and **Passed Out** are the exceptions — they fire on full-screen result/story cards while the gate is *off*. Load into the world, then trigger the states below.

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

### 1. In game (gate)  ·  `inGameWhite1`, `inGameWhite2`, `ingameNotWhite`

The white HUD element sets the in-game state: `inGameWhite1` ≥ .95 **and** `inGameWhite2` ≥ .95 with `ingameNotWhite` < .1 (or a visible health bar). It clears when either white reads < .1.

![in game](images/in_game.png)

### 2. Battle Start  ·  `battleRedBar`, `battleWhiteName`, `battleNotRed`

A battle begins: the enemy's **red HP bar** (`battleRedBar` ≥ .95) and **white name** (`battleWhiteName` ≥ .15) appear, guarded by `battleNotRed` < .1. Fires the **Battle Start** effect.

![battle](images/battle.png)

### 3. Battle Won  ·  `battleWonOrange`, `wonOrange`, `wonNotOrange`

The orange **"Battle Won"** victory banner appears after a battle: `battleWonOrange` ≥ .4 **and** `wonOrange` ≥ .4 with `wonNotOrange` < .15. **Fires while the gate is OFF.** Plays the **Win** effect.

![battle won](images/battle_won.png)

### 4. Capture  ·  `captureAniimoWhite`, `captureWhiteP`, `captureNotWhite`

Catching an aniimo: the creature's **name** (`captureAniimoWhite` between .11 and .4) and the **"P" prompt** (`captureWhiteP` ≥ .95) appear, guarded by `captureNotWhite` < .1. Fires the **Capture** effect.

![capture](images/capture_old_aniimo.png)

### 5. Water  ·  `waterBlue`, `waterWhite`

Entering water: the blue drop + white highlight on the **swim icon** (`waterBlue` ≥ .95 **and** `waterWhite` ≥ .95). A lingering effect that holds while you swim.

![in the water](images/in_the_water.png)

### 6. Level Up  ·  `levelWhite`, `levelblue`, `levelNotWhite`

A level-up card: the **level number** (`levelWhite` ≥ .25) and the **blue bar** (`levelblue` between .2 and .4), guarded by `levelNotWhite` < .1. Fires the **Level Up** effect.

![level](images/level.png)

### 7. Damage / Low Health  ·  `healthGreen1`, `healthGreen2`, `healthWhite`

The green **health bar** reads as two halves (`healthGreen1`, `healthGreen2`, averaged to HP), with `healthWhite` confirming the HUD is present.

* **Damage** — HP *drops* by ≥ .1 since the previous reading, while HP > .1 (change-based). Fires once per drop; a short cooldown dedupes the two bar halves draining in sequence.
* **Low Health** — HP ≤ .1 (and > .02). A lingering warning.

Damage is change-based, so validate it **live** rather than from a static frame. (Note: because the health meter only registers once the bar settles for a few frames, a slow continuous drain / damage-over-time may not register as a discrete hit.)

![low health](images/low_health.png)

### 8. New Story  ·  `newStoryYellow`, `newStoryWhite`, `newStoryWhite2`, `newStoryNotWhite`

A new chapter card: the **yellow star** (`newStoryYellow` ≥ .95) + **white text/circle** (`newStoryWhite` ≥ .2, `newStoryWhite2` ≥ .2), guarded by `newStoryNotWhite` < .1. **Fires while the gate is OFF.** Plays the **New Story** effect.

![new story](images/new_story.png)

### 9. Passed Out  ·  `passedOutwhite1`, `passedOutWhite2`, `passedOutNotWhite`

The defeat screen: the **retry + exit banners** (`passedOutwhite1` ≥ .95 **and** `passedOutWhite2` ≥ .95), guarded by `passedOutNotWhite` < .1. **Fires while the gate is OFF.** A lingering effect.

![passed out](images/passed_out.png)

### 10. Twine / Untwine  ·  `twineWhiteText`, `twineWhite2`, `untwineWhite2`

One shared prompt text gates it (`twineWhiteText` ≥ .15); `twineWhite2` and `untwineWhite2` swap to say which state you're in. The effect fires on a **transition**:

* untwined → twined (`twineWhite2` > .7) → **Untwine** effect
* twined → untwined (`untwineWhite2` > .7) → **Twine** effect

Twine:

![twine](images/twine.png)

Untwine:

![untwine](images/untwine.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (white HUD) | `inGameWhite1`, `inGameWhite2`, `ingameNotWhite` |
| Battle Start (enemy HP bar + name) | `battleRedBar`, `battleWhiteName`, `battleNotRed` |
| Battle Won (victory banner) | `battleWonOrange`, `wonOrange`, `wonNotOrange` |
| Capture (name + "P" prompt) | `captureAniimoWhite`, `captureWhiteP`, `captureNotWhite` |
| Water (swim icon) | `waterBlue`, `waterWhite` |
| Level Up (level card) | `levelWhite`, `levelblue`, `levelNotWhite` |
| Health bar (Damage + Low Health) | `healthGreen1`, `healthGreen2`, `healthWhite` |
| New Story (chapter card) | `newStoryYellow`, `newStoryWhite`, `newStoryWhite2`, `newStoryNotWhite` |
| Passed Out (defeat screen) | `passedOutwhite1`, `passedOutWhite2`, `passedOutNotWhite` |
| Twine / Untwine (prompt) | `twineWhiteText`, `twineWhite2`, `untwineWhite2` |

**30 meters across 10 screens** driving 11 effects.

---

## Notes

* **Two gate paths** — the white HUD element *or* a visible health bar sets the in-game state. **Battle Won, New Story and Passed Out fire while the gate is OFF** (full-screen result/story cards).
* **Change-based Damage** — Damage fires on the HP *drop* since the last reading (`prevHp - hp ≥ .1`), not on the meter's `decreased`/`diff` flags (those persist across unrelated `healthWhite`/other-half changes and caused repeat/phantom triggers). A 20-frame cooldown (`damageCool`) dedupes the two bar halves. Validate live.
* **Twine/Untwine are transition effects** — they only fire when the state flips (tracked by `twineState`), not on every frame the prompt is visible.
* **Capture name is a bounded range** — `captureAniimoWhite` must be between .11 and .4 (enough white to be the name, not so much it's a different prompt).
* **Capture status** — **complete**: all 30 meters captured across all 8 targets (7 SDR resolutions + 3840x2160 HDR) = 240 images, 0 remaining. See `meters.txt` / `Aniimo_test_images.txt`.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

# Helldivers 2

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

The integration also carries aspect-ratio fallbacks (**3.55:1**, **24:10** and **16:11**), so other resolutions sharing those aspect ratios are read from the nearest matching profile. **2560x1080** and **3840x2160** also have their own dedicated profiles. HDR color ranges are defined for every meter.

---

## Resolution Setup

Change the monitor resolution, launch the game and open the display/graphics settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display mode so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Eliminated Effect**, **Stim Effects**, **Mission Complete Effect**, **Crisis Effect**, **Killstreak Effect**, **Low Resource Effect**, **Reinforcement Effect**, **Low Reinforcement Effect** and **Low Time Effect**. **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

The **Health** and **Stamina** HUD bars each have on/off, color, position and size controls. Turn on **Adjustment Toggle** to show both bars at full size so they can be placed on the keyboard, and **turn it off in game**.

Most effects only fire while the integration considers you **in game** (see step 1). The exceptions are the **Loading** screen and the **Mission Complete** success card, which fire while the in-game state is *off*.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

Test images live in `SDR/<meter>/<meter>_<op><value>.png` (see `metters.txt` for the exact filenames).

### 1. In game (gate)  ·  `inGameBracketW`, `inGameBracketR`, `inGameBracketNotWR`

The in-game state turns on when the HUD bracket next to the stratagem/health area reads white or red: `inGameBracketW` + `inGameBracketR` >= .95 with `inGameBracketNotWR` <= .1. It turns off when the combined bracket value drops to <= .1. **Everything below requires this state** unless noted otherwise.

![in game](images/in_game.png)

### 2. Health / Low Health  ·  `healthWhite`, `healthRed`

The health bar (bottom-left) fill, read from the white and red parts together (`healthWhite` + `healthRed`). It drives the **Health HUD** bar. At <= 25% the **Low Resource** effect fires (health variant).

![low health](images/low_health.png)

### 3. Stamina / Low Stamina  ·  `staminaWhite`, `staminaConfirm`, `staminaNotConfirm`

The stamina bar (bottom-center) fill drives the **Stamina HUD** bar. The bar is only shown while stamina is in use, so `staminaConfirm` >= .95 with `staminaNotConfirm` <= .1 confirms it is visible; stamina is hidden otherwise. With the bar confirmed, stamina between .05 and .25 fires the **Low Resource** effect (stamina variant).

![low stamina](images/low_stamina.png)

### 4. Enemy faction  ·  `bugWhite`, `botWhite`, `alienWhite`, `bugNotWhite`

The enemy-faction icon at the top-right picks which **Eliminated** variant plays. The three meters read the same spot and `bugNotWhite` guards it (<= .1):

* **Terminids (bugs)** — `bugWhite` >= .95 while `alienWhite` and `botWhite` <= .1
* **Automatons (bots)** — `botWhite` >= .95 while `bugWhite` and `alienWhite` <= .1
* **Illuminate (aliens)** — `alienWhite` >= .95 **and** `botWhite` >= .95 while `bugWhite` <= .1

Until a faction is detected, the generic Eliminated effect is used.

Bug:

![bug](images/enemy_bug.png)

Bot:

![bot](images/enemy_bot.png)

Alien:

![alien](images/enemy_alien.png)

### 5. Eliminated  ·  `eliminatedRed1`, `eliminatedRed2`, `eliminatedNotRed`

The red **eliminated** banner appears in the center of the screen: `eliminatedRed1` >= .3 **and** `eliminatedRed2` >= .3 with `eliminatedNotRed` <= .1. Plays the **Eliminated** effect for the detected faction (see step 4).

![eliminated](images/eliminated.png)

### 6. Reinforce  ·  `reinforceRed`, `reinforceNotRed`

The red reinforcement indicator at the top-left: `reinforceRed` >= .95 with `reinforceNotRed` <= .1. Plays the **Reinforcement** effect.

![reinforce](images/reinforce.png)

### 7. Low Reinforcements  ·  `lowReinforceRed`

The reinforcement counter turns red when reserves run low: `lowReinforceRed` >= .95. Plays the **Low Reinforcement** effect. It has a **2 minute cooldown** between triggers.

![low reinforce](images/low_reinforce.png)

### 8. Killstreak  ·  `killstreakYellow`, `killstreakNotYellow`

The yellow killstreak text under the crosshair: `killstreakYellow` >= .95 with `killstreakNotYellow` <= .1. Plays the **Killstreak** effect, which stays up while the text is visible.

![killstreak](images/killstreak.png)

### 9. Crisis  ·  `crisisRed`

The red alert at the top-center of the screen: `crisisRed` >= .6. Plays the **Crisis** effect, which stays up until the alert clears (<= .1).

![crisis](images/crisis.png)

### 10. Mission area complete  ·  `missionYellow`, `missionCyan`, `missionNotYC`

The mission-update card in the upper center: the yellow and cyan elements (`missionYellow` >= .95 **and** `missionCyan` >= .95) with `missionNotYC` <= .1. Plays the **Mission Complete** effect while in game.

![mission area](images/mission_area.png)

### 11. Mission success  ·  `missionCompleteYellow`

The yellow title on the end-of-mission success card: `missionCompleteYellow` >= .95. **Fires while the in-game state is OFF.** Plays the same **Mission Complete** effect.

![mission complete](images/mission_complete.png)

### 12. Low Time  ·  `timeRed`

The mission timer at the top-right turns red when time is running out: `timeRed` >= .1. Plays the **Low Time** effect, with a **60 second cooldown** between triggers.

![low time](images/low_time.png)

### 13. Stims  ·  `stimWhite`, `stimRed`, `stimUnit`

The stim icon and counter at the bottom-left:

* **Stim use** — `stimWhite` >= .95 records the starting stim count (`stimUnit`, read by OCR, valid values 2-4). When the count later changes, the **Stim** effect fires.
* **Low stims** — `stimRed` >= .95 (the icon turns red). Plays the **Low Stims** effect.

Stim use is change-based, so validate it **live** rather than from a static frame.

Stim:

![stim](images/stim.png)

Low stims:

![low stims](images/stim_low.png)

### 14. Loading screen  ·  `loadGrey`, `loadBlack`

The loading indicator at the bottom-right corner: `loadGrey` >= .95 **and** `loadBlack` >= .95. **Fires while the in-game state is OFF.** Plays the **Loading** effect.

![loading](images/loading.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (HUD bracket) | `inGameBracketW`, `inGameBracketR`, `inGameBracketNotWR` |
| Health (Health HUD + Low Health) | `healthWhite`, `healthRed` |
| Stamina (Stamina HUD + Low Stamina) | `staminaWhite`, `staminaConfirm`, `staminaNotConfirm` |
| Eliminated (banner) | `eliminatedRed1`, `eliminatedRed2`, `eliminatedNotRed` |
| Enemy faction (top-right icon) | `bugWhite`, `botWhite`, `alienWhite`, `bugNotWhite` |
| Reinforce | `reinforceRed`, `reinforceNotRed` |
| Low Reinforcements | `lowReinforceRed` |
| Killstreak | `killstreakYellow`, `killstreakNotYellow` |
| Crisis | `crisisRed` |
| Mission area complete | `missionYellow`, `missionCyan`, `missionNotYC` |
| Mission success card | `missionCompleteYellow` |
| Low Time | `timeRed` |
| Stims | `stimWhite`, `stimRed`, `stimUnit` |
| Loading screen | `loadGrey`, `loadBlack` |

**31 meters across 14 screens** driving 11 effects (plus the Health and Stamina HUD bars).

---

## Notes

* **In-game detection gates most effects.** The exceptions are **Loading** and the **Mission success** card, which only fire while the gate is OFF.
* **Positive / negative pairs** — most states use a positive meter plus a `…Not…` negative guard (e.g. `killstreakYellow` + `killstreakNotYellow`). Both must read correctly for the effect to fire cleanly.
* **Eliminated depends on the faction** — the enemy icon is read first, so make sure the faction is detected (step 4) before validating Eliminated.
* **Cooldowns** — Low Time (60 s) and Low Reinforcements (2 min) will not re-trigger inside their cooldown, so wait before retesting.
* **Stim use is change-based** — it fires when the stim count changes after the initial count is read, not on every frame the icon is visible.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

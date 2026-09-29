# Arc Raiders

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Screen resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every event has its own on/off toggle in the SignalRGB UI (Low Health, Healing, Downed, Damage, Sprint, Encumbered, XP, Victory/Defeat, Looting).

The **Health Bar** and **Shield Bar** are persistent HUD bars, each with its own position and size controls (Health/Shield Bar X / Y / Width / Height). Use **Adjust Health / Shield Bar** to draw both bars at full size while out of game so you can position them on your devices, then turn it off before testing.

The background is controlled separately by **Background Mode** (Ambience by default), **Background Brightness** and **Custom Color** — these are ambience, not event effects.

---

## Testing Guidelines

The in-game gate is the white **compass** across the **top-center** of the screen (`inGameWhiteCompass`) together with a health/shield bar being present. Almost every effect requires the gate; the looting screen is the exception — it fires while the compass is *gone*. Drop into a raid, then trigger the states below.

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter. The bottom-left health/shield HUD alone carries **18 meters** (three segments each of health, shield, and their red/yellow variants); sprint, loot and the others group too.

### 1. In game  ·  `inGameWhiteCompass`

Drop into a raid. The white **compass** strip runs across the top-center (NW · N · NE · E). `inGameWhiteCompass` (≥ .15) — with a health/shield bar present — sets the in-game state that gates every other effect; it clears when the compass drops below .05 and health is gone.

![inGame](images/inGame.png)

### 2. Health & Shield bars (Damage / Heal / Low health / Downed)  ·  `health1-3`, `shield1-3`, `healthRed1-3`, `shieldRed1-3`, `healthYellow1-3`, `shieldYellow1-3`

The segmented health (white) and shield (blue) bars sit at the **bottom-left**. Each bar is read in **three segments** and averaged: `health1/2/3` → Health, `shield1/2/3` → Shield (these drive the persistent **Health Bar** and **Shield Bar** HUDs). The same segments are also read for red (`healthRed1-3` / `shieldRed1-3`, a damage flash) and yellow (`healthYellow1-3` / `shieldYellow1-3`, a heal flash). From these the engine derives:

* **Damage** — Health/Shield *drops* with red present.
* **Healing** — yellow present on either bar.
* **Low Health** — Health between ~.01 and .25.
* **Downed** — red high (≥ .5) while Health has bottomed out (≤ .05).

All 18 segment meters read the one bottom-left HUD region, so **one screenshot covers them all**.

![healthShield](images/healthShield.png)

### 3. Sprint  ·  `sprint1`, `sprint2`, `sprintNotGrey`, `sprintValue`

Sprint so the stamina bar appears (center, just below the crosshair). `sprint1` (left end) and `sprint2` (right end) both read ≥ .95 with `sprintNotGrey` (≤ .05) guarding, to confirm the bar is on screen; `sprintValue` reads how full it is (0–1) and drives the sprint effect's color (green → red as it drains). Triggers the **Sprint** effect.

![sprint](images/sprint.png)

### 4. Encumbered  ·  `heavyBlack`, `heavyRed`

Carry too much weight so the overweight/encumbered indicator appears (center-bottom, near the weight readout). `heavyBlack` (≥ .95) + `heavyRed` (≥ .95) trigger the **Encumbered** effect.

![encumbered](images/encumbered.png)

### 5. XP  ·  `xpYellow`, `xpBlack`

Gain experience so the XP indicator appears on the **left edge**. `xpYellow` (≥ .95) + `xpBlack` (≥ .25) trigger the **XP** effect.

![xp](images/xp.png)

### 6. Looting  ·  `lootWhite`, `lootNotWhite`, `lootBlue`

Open a container / the inventory screen. **This is read while out of the field HUD** — `inGameWhiteCompass` must be below .05 (no compass on the loot screen). `lootWhite` (≥ .25) + `lootBlue` (≥ .95) with `lootNotWhite` (≤ .05) guarding trigger the **Looting** effect.

![loot](images/loot.png)

### 7. Victory / Extraction  ·  `winGreen`

Successfully extract. The green **RETURNING TO SPERANZA** banner fills the screen. `winGreen` (≥ .95), with Health gone (≤ .05, you have left the field), plays the **Victory** effect.

![victory](images/victory.png)

### 8. Defeat  ·  `lossOrangeRed`

Die / fail the raid. The orange-red result banner appears in the same region as the victory banner. `lossOrangeRed` (≥ .95), with Health gone (≤ .05), plays the **Defeat** effect.

![defeat](images/defeat.png)

---

## Meter → Image map

| Image | Meters on it |
| --- | --- |
| `inGame.png` | `inGameWhiteCompass` |
| `healthShield.png` | `health1`, `health2`, `health3`, `shield1`, `shield2`, `shield3`, `healthRed1-3`, `shieldRed1-3`, `healthYellow1-3`, `shieldYellow1-3` |
| `sprint.png` | `sprint1`, `sprint2`, `sprintNotGrey`, `sprintValue` |
| `encumbered.png` | `heavyBlack`, `heavyRed` |
| `xp.png` | `xpYellow`, `xpBlack` |
| `loot.png` | `lootWhite`, `lootNotWhite`, `lootBlue` |
| `victory.png` | `winGreen` |
| `defeat.png` | `lossOrangeRed` |

That is **8 images for all 32 meters** — the health/shield HUD groups 18 meters into one frame.

---

## Notes

* **In-game detection gates most effects** — health/shield, sprint, encumbered, XP and downed all require the compass gate. **Looting is the exception** (compass must be *off*), and Victory/Defeat are read as you leave the field (Health ≤ .05).
* **Health & Shield are three-segment averages** — each bar's value is `(seg1 + seg2 + seg3) / 3`. The red (damage) and yellow (heal) meters read the *same* three segments, so damage, healing, low-health and downed all derive from the one bottom-left region.
* **Change-based meters** — Damage fires on the Health/Shield bar *dropping* (with red present), not at a fixed level. Validate it by taking damage live rather than from a static frame.
* **`sprintValue` is a fill meter** — it reads how full the stamina bar is (0–1) and colors the sprint effect; the test images use exact-value names (`sprintValue_v.67_t10`, etc.).
* Detection strategy: positive meters (expected color present) are paired with negative "Not" guards (`sprintNotGrey`, `lootNotWhite`). A state is active only when the positive meters are high and the negative guard is low.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

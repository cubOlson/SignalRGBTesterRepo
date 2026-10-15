# Doom: The Dark Ages

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

Change the monitor resolution, launch the game and open the display/video settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display mode so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Health Effects**, **Armor Effects**, **Low Ammo Effect**, **Shield Block Effect**, **Death Effects**, **Boss Effect** and **Gold Effect**. **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

Most effects only fire while the integration considers you **in game** (see step 1). The exceptions are the **Death** and **Life Sigil** effects, which fire while the in-game state is *off*.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

Test images live in `SDR/<meter>/<meter>_<op><value>.png`.

### 1. In game (gate)  ·  `inGameWhite1`, `inGameWhite2`, `inGameNotWhite`

The in-game state turns on when the white HUD element at the bottom-left (the ability wheel) reads: `inGameWhite1` >= .95 **and** `inGameWhite2` >= .95 with `inGameNotWhite` <= .05. It turns off when either white meter drops to <= .25. **Most effects require this state.** All three meters use the same screenshot.

![in game](images/in_game.png)

### 2. Shield Block  ·  `shieldGreen`, `shieldRed`

The shield icon at the bottom-left changes color with its state. `shieldRed` >= .5 fires the **Shield Block** effect (a red burst of spikes sweeps across the keyboard). It re-arms once the icon returns to its ready state (`shieldGreen` >= .5).

Ready (green):

![shield ready](images/shield_ready.png)

Block (red):

![shield block](images/shield_block.png)

### 3. Health bar  ·  `healthBlue`, `healthRed`

The health number at the bottom-center. `healthBlue` >= .95 is the normal state; `healthRed` >= .95 means health has dropped into the red and arms the **Critical Health** effect (step 4). Returning to blue clears it.

Normal:

![health](images/health.png)

Low:

![low health](images/low_health.png)

### 4. Critical Health  ·  `healthCriticalRed`, `healthNotCriticalRed`

The **CRITICAL HEALTH** label above the health bar: `healthCriticalRed` > 0 with `healthNotCriticalRed` <= .05, while health is in the red state (step 3). Plays the **Critical Health** effect (red blood drops spray from the center) and keeps going until health recovers (`healthBlue` >= .5).

![critical health](images/critical_health.png)

### 5. Health Up  ·  `healthIncreaseWhite`, `healthIncreaseNotWhite`

The white **+** pickup indicator under the health number: `healthBlue` >= .95 **and** `healthIncreaseWhite` >= .95 with `healthIncreaseNotWhite` <= .05. Plays the **Health Up** effect (a blue wash with a white line sweeping up the screen).

![health up](images/health_up.png)

### 6. Armor  ·  `armorGreen`, `armorNotGreen`

The green armor number at the bottom-center.

* **Armor gained** — `armorGreen` >= .95 with `armorNotGreen` <= .05 after the armor was empty. Plays the **Armor Up** effect (a green wash with a white line sweeping up the screen).
* **Armor broken** — `armorGreen` drops to 0 after being active. Plays the **Armor Broken** effect (red ripple rings).

Armor broken is change-based, so validate it **live** rather than from a static frame.

![armor](images/armor.png)

### 7. Armor Up  ·  `armorIncreaseWhite`, `armorIncreaseNotWhite`

The white **+** pickup indicator under the armor number: `armorGreen` >= .95 **and** `armorIncreaseWhite` >= .95 with `armorIncreaseNotWhite` <= .05. Plays the same **Armor Up** effect on every armor pickup.

![armor up](images/armor_up.png)

### 8. Low Ammo  ·  `lowAmmoRed`, `lowAmmoNotRed`

The ammo counter at the bottom-right turns red: `lowAmmoRed` >= .1 with `lowAmmoNotRed` <= .1. Plays the **Low Ammo** effect (a red pulse with bars closing in, repeated twice).

![low ammo](images/low_ammo.png)

### 9. Boss  ·  `leaderRed`, `leaderPurple`

The boss health bar at the top-center: the red health bar (`leaderRed` >= .5) and the purple stagger bar marker (`leaderPurple` >= .5). Plays the **Boss** effect: a red pulse fills the keyboard from the left, sized to the boss's remaining health, and ends when the bar disappears (both <= .01). Both meters use the same screenshot.

![boss](images/boss.png)

### 10. Gold  ·  `goldYellow`, `goldNotYellow`

The yellow gold icon at the top-right appears when gold is picked up: `goldYellow` >= .95 with `goldNotYellow` <= .05. Plays the **Gold** effect (a yellow glow expands from the center and fades). The gold icon is visible in the top-right of the boss screenshot above; there is no dedicated gold test image yet.

![gold](images/boss.png)

### 11. Life Sigil  ·  `sigilGreen`, `sigilNotGreen`, `sigilConfirm`

The **"You have died. Would you like to use a Life Sigil?"** prompt: the green sigil (`sigilGreen` >= .95) with `sigilNotGreen` <= .05, confirmed by the dark screen edge (`sigilConfirm` >= .95). **Fires while the in-game state is OFF.** Plays the **Life Sigil** effect (a green glow expands from the center). It ends when the prompt is dismissed.

![life sigil](images/life_sigil.png)

### 12. Death  ·  `deathRed`, `deathNotRed`, `sigilConfirm`

The **YOU HAVE DIED** screen: the red skull (`deathRed` >= .95) with `deathNotRed` <= .05, confirmed by the same `sigilConfirm` dark-edge meter. **Fires while the in-game state is OFF.** Plays the **Death** effect (a red wash with jaws of teeth snapping open and shut). It ends when the screen is dismissed.

![death](images/death.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (ability wheel) | `inGameWhite1`, `inGameWhite2`, `inGameNotWhite` |
| Shield (ready / block) | `shieldGreen`, `shieldRed` |
| Health bar (normal / low) | `healthBlue`, `healthRed` |
| Critical Health | `healthCriticalRed`, `healthNotCriticalRed` |
| Health pickup | `healthIncreaseWhite`, `healthIncreaseNotWhite` |
| Armor (gain / break) | `armorGreen`, `armorNotGreen` |
| Armor pickup | `armorIncreaseWhite`, `armorIncreaseNotWhite` |
| Low Ammo | `lowAmmoRed`, `lowAmmoNotRed` |
| Boss health bar | `leaderRed`, `leaderPurple` |
| Gold pickup | `goldYellow`, `goldNotYellow` |
| Life Sigil prompt | `sigilGreen`, `sigilNotGreen`, `sigilConfirm` |
| Death screen | `deathRed`, `deathNotRed` (+ shared `sigilConfirm`) |

**26 meters across 12 screens** driving 10 effects.

---

## Notes

* **In-game detection gates most effects.** The exceptions are **Life Sigil** and **Death**, which only fire while the gate is OFF (full-screen prompts).
* **Positive / negative pairs** — most states use a positive meter plus a `…Not…` negative guard (e.g. `lowAmmoRed` + `lowAmmoNotRed`). Both must read correctly for the effect to fire cleanly.
* **Critical Health needs the red state first** — it only fires after `healthRed` has been seen, so reduce health to the red before checking the critical label.
* **Armor broken and Shield Block are state-based** — they fire on the transition (armor empties, shield icon turns red) and re-arm when the state returns to normal, so validate them **live**.
* **Shared screenshots** — several meters read the same screen and share one image (for example `shieldGreen` and `healthRed`, or `armorGreen` and `healthBlue`). `sigilConfirm` is reused by both the Life Sigil and Death effects.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

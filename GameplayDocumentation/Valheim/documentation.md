# Valheim

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

SDR and HDR (HDR tuned for 3840x2160)

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display settings so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every event has its own on/off toggle in the SignalRGB UI: **Craft**, **Low Health**, **Damage**, **Healing**, **Portal**, **Low Stamina**, **Sleep**, **Death**.

The background is controlled separately by **Background Mode** (Ambience by default), **Background Brightness** and **Custom Color** — these are ambience, not event effects.

---

## Testing Guidelines

The in-game gate is a small **red HUD element at the bottom-left** (`inGameRed` ≥ .8, guarded by `inGameNotRed` < .1). In-world effects require the gate; **Portal** and **Sleep** are the exceptions — they fire on loading/transition screens while the gate is *off*. Load a character, then trigger the states below.

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter. The crafting window carries 3 meters, the portal screen 2, the sleep prompt 2.

### 1. In game (gate)  ·  `inGameRed`, `inGameNotRed`

Load into the world. The bottom-left player HUD shows the vertical **health bar** (red) and **stamina bar**, plus the small red marker the gate reads. `inGameRed` (≥ .8) with `inGameNotRed` (< .1) sets the in-game state that gates the in-world effects; it clears when `inGameRed` drops below .1. This frame also carries the health/stamina meters (bottom-left HUD).

![in game](images/inGame.png)

### 2. Health — Damage / Healing / Low Health / Death  ·  `lifeRed`, `lifeNotRed`, `deathText`

The health bar is the **red vertical bar on the left edge** (`lifeRed`, guarded by `lifeNotRed`). It drives four effects:

* **Damage** — `lifeRed` *decreases* by ≥ .08 (change-based). Take a hit.
* **Healing** — `lifeRed` *increases* by ≥ .08 (change-based). Eat food / rest.
* **Low Health** — `lifeRed` ≤ .2 with `lifeNotRed` < .1.
* **Death** — `lifeRed` < .1 **and** the yellow **"You died"** text (`deathText` ≥ .25) appears center-screen.

Because Damage and Healing are change-based they must be validated **live**, not from a static frame. Note: a *full* Valheim health bar only fills ~0.3 of the meter region, so the "healthy" capture is `lifeRed_gte.2` (not `.9`); `lifeRed_lt.2` is the low-health/death state.

Low health (health bar nearly empty):

![low health](images/lowHealth.png)

Death — the yellow **"YOU DIED!"** text center-screen (`deathText` ≥ .25) with health gone:

![death](images/death.png)

### 3. Stamina — Low Stamina  ·  `yellowStamina`, `staminaNotYellow`

The stamina bar is the **yellow bar at the bottom-center** (`yellowStamina`, guarded by `staminaNotYellow`). When it nearly empties (`yellowStamina` ≤ .25 with `staminaNotYellow` < .1) the **Low Stamina** effect fires. Sprint/jump to drain it; the capture `yellowStamina_lt.25` shows the near-empty bar.

![low stamina](images/lowStamina.png)

### 4. Craft  ·  `craftWhite`, `craftBrown`, `craftOrange`

Open the crafting station window. `craftWhite` (≥ .15) and `craftBrown` (≥ .2) confirm the window is open, and `craftOrange` (≥ .95) marks the craft progress/confirm state. All three read the crafting panel, so **one screenshot covers them**. Triggers the **Craft** effect.

![craft](images/craft.png)

### 5. Portal  ·  `portalRed`, `portalDark`

Enter a portal. On the teleport/transition screen a swirling orange-red ring (`portalRed` ≥ .95) surrounds a dark purple center (`portalDark` ≥ .5). **This fires while the in-game gate is OFF** (`gInGame` == false). Both meters read the same screen. Triggers the **Portal** effect.

![portal](images/portal.png)

### 6. Sleep  ·  `sleepYellow`, `sleepGray`

Sleep in a bed. The screen fades to black with the yellow **"Day N" / "ZZZZZzzzz…"** sleep text (`sleepYellow` ≥ .2) and a gray element bottom-right (`sleepGray` ≥ .5). **This also fires while the gate is OFF.** Both read the same screen. Triggers the **Sleep** effect. *(Note: the "ZZZ" text is dim — the trigger was tuned to .2 to catch it reliably.)*

![sleep](images/sleep.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (bottom-left HUD) | `inGameRed`, `inGameNotRed` |
| Health bar (left edge) + death text | `lifeRed`, `lifeNotRed`, `deathText` |
| Stamina bar (bottom-center) | `yellowStamina`, `staminaNotYellow` |
| Crafting window | `craftWhite`, `craftBrown`, `craftOrange` |
| Portal loading screen | `portalRed`, `portalDark` |
| Sleep prompt | `sleepYellow`, `sleepGray` |

**14 meters across 6 screens.**

---

## Notes

* **In-game gate** — `inGameRed` (≥ .8) / `inGameNotRed` (< .1) gate Craft, Health (damage/healing/low/death) and Stamina. **Portal and Sleep fire while the gate is OFF** (loading/transition screens).
* **Change-based effects** — Damage and Healing have no dedicated meter; they read `lifeRed` *changing* (decrease → Damage, increase → Healing, diff ≥ .08). Validate them live.
* **Low Health vs Death** — both read `lifeRed` low; Death additionally requires the yellow `deathText` banner (≥ .25). Low Health self-clears if `lifeRed` ≤ .02 (dead) so it doesn't stack with Death.
* **Effect suppression** — Low Health and Low Stamina are suppressed while the Death effect is active (`deathGoing`).
* **Capture status** — all 15 captures are present for every resolution (7 × SDR + 3840x2160 HDR = 120 PNGs), sorted into per-meter folders. The `3840x1080` `lifeRed` frame was taken on a crafting screen rather than normal gameplay — worth re-capturing for a cleaner health-bar reference.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

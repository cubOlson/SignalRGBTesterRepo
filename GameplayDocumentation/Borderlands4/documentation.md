# Borderlands 4

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

SDR and HDR

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Screen resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every event has its own on/off toggle in the SignalRGB UI (Shield Break, Combat, Level Up, Low Health / Fight-For-Your-Life, Death).

The **Health Bar** and **Shield Bar** are persistent HUD bars, each with its own position and size controls (Health/Shield Bar X / Y / Width / Height). Use **Adjust Health / Shield Bar Position** to draw both bars while out of game so you can position them on your devices, then turn it off before testing.

The background is controlled separately by **Background Mode** (Ambience by default), **Background Brightness** and **Custom Color** — these are ambience, not event effects.

---

## Testing Guidelines

The in-game gate reads two places: the objective marker on the **compass** at the **top-center** (`inGameColor`) and the white player HUD at the **bottom-left** (`inGameWhite`, guarded by `inGameNotWhite`). Almost every effect requires the gate. A separate **not-sprinting** gate (`noSprintLeft` + `noSprintRight`, the ends of the bottom-left bars) must also be satisfied for the screen-filling effects — while sprinting the HUD changes and those effects are suppressed. Load into a level, then trigger the states below.

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter. The standard gameplay HUD alone carries the gate plus the health, shield, XP, skill and sprint meters (10 meters); combat, FFYL and jetpack group too.

### 1. In game + Health / Shield / XP / Skill / Sprint HUD  ·  `inGameColor`, `inGameWhite`, `inGameNotWhite`, `healthRed`, `shieldBlue`, `shieldYellow`, `xpLevel`, `skillPoint`, `noSprintLeft`, `noSprintRight`

Load into a level. The bottom-left HUD shows the **level badge** (with the yellow **XP** arc around it → `xpLevel`, and the **skill-point** icon → `skillPoint`), the blue **shield** bar (`shieldBlue`, or `shieldYellow` for a yellow/absorb shield) and the red **health** bar (`healthRed`). The white HUD elements drive the gate (`inGameWhite` ≥ .2, `inGameNotWhite` ≤ .1), together with the top-center objective marker (`inGameColor` ≥ .95, or `healthRed` ≥ .25 as a fallback). The bar ends (`noSprintLeft` / `noSprintRight`, combined ≥ 1.75) confirm the HUD is up and you are **not sprinting**.

From these: `healthRed` drives the **Health Bar** HUD and, when low, the **Low Health** vignette; `shieldBlue` / `shieldYellow` drive the **Shield Bar** HUD and, when both drop out, the **Shield Break** effect; `xpLevel` resetting drives **Level Up**; `skillPoint` drives the **Skill Point** effect. All ten read this one HUD, so **one screenshot covers them all**.

![inGame](images/inGame.png)

### 2. Combat  ·  `combatRed`, `combatNotRed`, `miniCombatRed`, `miniCombatNotRed`, `miniConfirmYellow`

When you enter combat the compass line at the **top-center** turns red (`combatRed` ≥ .95, guarded by `combatNotRed` ≤ .1), and a red combat indicator appears on the **right edge** by the mini-map (`miniCombatRed` ≥ .1, guarded by `miniCombatNotRed` ≤ .05). `miniConfirmYellow` confirms the return to a peaceful state. Entering combat plays the **In Combat** effect; leaving it plays the **Out of Combat** effect.

![combat](images/combat.png)

### 3. Fight For Your Life / Death  ·  `ffylRed`, `ffylBlack`, `ffylLength`

When you are downed, the red **LUTE PELA VIDA! / FIGHT FOR YOUR LIFE** banner appears in the center. `ffylRed` (≥ .95) + `ffylBlack` (≥ .95) with `ffylLength` (≥ .05, the countdown bar) trigger the **Fight-For-Your-Life** effect. If the FFYL state ends with you out of the game (the timer ran out), the **Death** effect plays instead.

![ffyl](images/ffyl.png)

### 4. Jetpack  ·  `jetPackLeft`, `jetPackRight`, `jetPackNotYellow`, `jetPackValue`

Use the jetpack so the fuel bar appears (center, below the crosshair). `jetPackLeft` (≥ .95) + `jetPackRight` (≥ .95) with `jetPackNotYellow` (≤ .1) confirm the bar is on screen, and `jetPackValue` (≥ .05) reads how much fuel remains and drives the effect. Triggers the **Jetpack** effect.

![jetpack](images/jetpack.png)

---

## Meter → Image map

| Image | Meters on it |
| --- | --- |
| `inGame.png` | `inGameColor`, `inGameWhite`, `inGameNotWhite`, `healthRed`, `shieldBlue`, `shieldYellow`, `xpLevel`, `skillPoint`, `noSprintLeft`, `noSprintRight` |
| `combat.png` | `combatRed`, `combatNotRed`, `miniCombatRed`, `miniCombatNotRed`, `miniConfirmYellow` |
| `ffyl.png` | `ffylRed`, `ffylBlack`, `ffylLength` |
| `jetpack.png` | `jetPackLeft`, `jetPackRight`, `jetPackNotYellow`, `jetPackValue` |

That is **4 images for all 22 meters** — the gameplay HUD groups ten meters into one frame.

---

## Notes

* **Two gates** — `inGameColor` + `inGameWhite`/`inGameNotWhite` set the in-game state, and `noSprintLeft` + `noSprintRight` set the *not-sprinting* state. The full-screen effects (low health, shield break, combat, level up) require **both** (they are suppressed while sprinting). Health/shield HUD bars and jetpack only need the in-game gate.
* **Shield can be blue or yellow** — `shieldBlue` and `shieldYellow` read the same shield bar for the two shield types; the Shield Break effect fires when **both** drop out.
* **Level up is a reset meter** — `xpLevel` drops to ~0 when the XP bar wraps on a level-up, then re-arms once it fills back to ≥ .95.
* **FFYL and Death are linked** — the FFYL banner (`ffylRed`+`ffylBlack`+`ffylLength`) starts the fight-for-your-life effect; if it ends out of game the Death effect plays.
* **Capture coverage / naming** — only **3840x2160 SDR** is captured so far, and five test images have **no comparison operator** in their filename (`jetPackLeft.png`, `jetPackRight.png`, `jetPackNotYellow.png`, `jetPackValue.png`, `shieldYellow.png`), so the framework will **skip** them. Per the effect code they should be `jetPackLeft_gte.95`, `jetPackRight_gte.95`, `jetPackNotYellow_lte.1`, `jetPackValue_gte.05`, and `shieldYellow_gte.9` (mirroring `shieldBlue_gte.9`). Other resolutions still need capturing.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

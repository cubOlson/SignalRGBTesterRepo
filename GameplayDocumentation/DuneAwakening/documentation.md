# Dune Awakening

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

Change the monitor resolution, launch the game and open the display settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display mode so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Each event group has its own on/off toggle in the SignalRGB UI: **Health HUD**, **Energy HUD**, **Health Effects** (low health + heal), **Shield Effects** (up + down), **Vitality Effects**, **Water Effects** (sheltered + exposed), **Vehicle**, **Level**, **Heat Effects** (warning + heatstroke), **Worm**, **Zone**.

The **Health HUD** and **Energy HUD** are persistent bars with their own position/size controls (Health/Energy HUD X / Y / Width / Height). Turn on **Adjust HUD** to draw both bars so you can place them on your devices, then turn it off to see the real effects.

The background is controlled separately by **Background Mode** (Ambience by default), **Background Brightness** and **Custom Color** — these are ambience, not event effects.

---

## Folder / Capture Layout

```
DuneAwakening/
  DuneAwakening/
    <resolution>/            e.g. 1920x1080, 2560x1440, 3840x2160 …
      SDR/
        <meter>/             one folder per meter
          <meter>_<op>.png   e.g. wormRed_gte.95.png
      HDR/                   (3840x2160 only)
        <meter>/
  DuneAwakening_test_images.txt   full list of capture filenames (meter_op)
  DuneAwakening_effects.txt       effect reference (name | description | trigger)
  DuneAwakeningDocumentation/
    documentation.md
    images/                  reference screenshots for this doc
```

The operator suffix encodes the state: `_gte.N` = value must be ≥ N (state present), `_lte.N` = value must be ≤ N (guard / state absent). See `DuneAwakening_test_images.txt` for the complete list; values come from the thresholds in `DuneAwakening.html`.

> **Capture status:** 22 of 30 meters are captured at **3840x2160 SDR**. Still missing: `inGameRed`, the four `ride*` meters, and the three `levelUp*` meters (8 total) — notably the same meters whose 4K overrides are broken (see Integration issues below). Other resolutions and HDR are not captured yet. The frame below is from `3840x2160/SDR`; it (and the other captures) include the on-screen **Screen Utility** capture-tool window, which is not part of the game.

---

## Testing Guidelines

The in-game gate reads the **zone indicator at the top-center** of the screen: one of `inGameYellow1` (PVE), `inGameGreen` (safe), or `inGameRed` (PVP) must be high **and** `inGameYellow2` high **and** `inGameNotYellow` low. Every effect requires the gate. Load into the world, then trigger the states below.

> **One screenshot can cover several meters.** The **bottom-left HUD cluster** (health, shield, energy, vitality, water) packs many meters into one frame; the top-center zone indicator carries 5; the center-screen warnings (level up, heat, worm) each share a frame.

### 1. In game (gate) + Zone  ·  `inGameYellow1`, `inGameYellow2`, `inGameGreen`, `inGameRed`, `inGameNotYellow`

The small zone indicator sits at the **top-center**. Its color encodes the zone: **yellow = PVE** (`inGameYellow1`), **green = safe** (`inGameGreen`), **red = PVP** (`inGameRed`); `inGameYellow2` is the confirm swatch just below and `inGameNotYellow` is the guard. Any of the three colors ≥ .95 (with `inGameYellow2` ≥ .95 and `inGameNotYellow` ≤ .05) sets the in-game state; all three ≤ .25 clears it. A change of zone color fires the **Zone** effect in that zone's color.

The frame below also shows the full HUD this doc refers to: the top-center compass/zone indicator, and the bottom-left status cluster (shelter bar + health/vitality/energy/water diamonds). *(`inGameRed` — the PVP variant — is not captured yet.)*

![in game + HUD](images/inGame.png)

*Reference:* `…/inGameYellow1/inGameYellow1_gte.95.png`, `…/inGameYellow2/inGameYellow2_gte.95.png`, `…/inGameGreen/inGameGreen_gte.95.png`, `…/inGameNotYellow/inGameNotYellow_lte.05.png`

### 2. Health + Shield (bottom-left HUD)  ·  `healthCheckRed`, `healthCheckNotRed`, `healthBarRed`, `shieldCyan`

The red **health bar** sits at the bottom-left. `healthCheckRed` (≥ .95, guarded by `healthCheckNotRed` ≤ .05) confirms the bar is present and drives the **Health HUD**; `healthBarRed` reads the fill. When `healthBarRed` ≤ .25 → **Low Health**; when it recovers past .25 → **Heal**. The **shield** indicator (`shieldCyan` ≥ .9) nearby drives **Shields Up**, and its disappearance drives **Shields Down**. The captured frame is the low-bar state (`healthBarRed_lte.25`).

Low health bar (bottom-left HUD):

![low health](images/health.png)

Shield active — note the cyan Holtzman-shield glow around the player (`shieldCyan`):

![shield](images/shield.png)

*Reference:* `…/healthCheckRed/healthCheckRed_gte.95.png`, `…/healthCheckNotRed/healthCheckNotRed_lte.05.png`, `…/healthBarRed/healthBarRed_lte.25.png`, `…/shieldCyan/shieldCyan_gte.9.png`

### 3. Energy (bottom-left HUD)  ·  `energyCheckYellow`, `energyCheckNotYellow`, `energyBarYellow`

The yellow **energy bar** sits just below health. `energyCheckYellow` (≥ .95, guarded by `energyCheckNotYellow` ≤ .05) drives the **Energy HUD**; `energyBarYellow` reads the fill. When ≤ .25 → **Low Energy**. The captured frame is the low-bar state (`energyBarYellow_lte.25`).

![low energy](images/energy.png)

*Reference:* `…/energyCheckYellow/energyCheckYellow_gte.95.png`, `…/energyCheckNotYellow/energyCheckNotYellow_lte.05.png`, `…/energyBarYellow/energyBarYellow_lte.25.png`

### 4. Vitality (bottom-left HUD)  ·  `vitalCheckGreen`, `vitalCheckNotGreen`, `vitalBarGreen`

The green **vitality bar**. `vitalCheckGreen` (≥ .95, guarded by `vitalCheckNotGreen` ≤ .05) + `vitalBarGreen` fill. When ≤ .25 → **Low Vitality**. The captured frame is the low-bar state (`vitalBarGreen_lte.25`).

![low vitality](images/vitality.png)

*Reference:* `…/vitalCheckGreen/vitalCheckGreen_gte.95.png`, `…/vitalCheckNotGreen/vitalCheckNotGreen_lte.05.png`, `…/vitalBarGreen/vitalBarGreen_lte.25.png`

### 5. Water / Shelter  ·  `waterBlue`, `waterRed`

The far-left status icon shows exposure to the sun. `waterBlue` ≥ .95 → **Sheltered**; `waterRed` ≥ .95 → **Exposed**. Both read the same icon (color differs).

Sheltered (blue — out of the sun; note the "SHELTERED/ENCLOSED" label):

![sheltered](images/sheltered.png)

Exposed (red — out in the open desert sun):

![exposed](images/exposed.png)

*Reference:* `…/waterBlue/waterBlue_gte.95.png`, `…/waterRed/waterRed_gte.95.png`

### 6. Vehicle  ·  `rideYellow`, `rideNotYellow`, `rideBlack`, `rideNotBlack`

When mounted, the vehicle module shows at the **bottom-right**. `rideBlack` ≥ .95 and the `rideYellow` fuel ≥ .95 (with `rideNotBlack` and `rideNotYellow` ≤ .05) trigger the **Vehicle** effect.

*Not captured yet* (`rideBlack_gte.95`, `rideYellow_gte.95`, `rideNotBlack_lte.05`, `rideNotYellow_lte.05`). ⚠ The `rideYellow`/`rideNotYellow` 4K region overrides are broken — see Integration issues.

### 7. Level Up  ·  `levelUpOrange`, `levelUpNotOrange`, `levelUpNotOrange2`

On a level-up, an orange banner appears center-upper. `levelUpOrange` ≥ .95 with both `levelUpNotOrange` and `levelUpNotOrange2` ≤ .05 fires the **Level Up** effect.

*Not captured yet* (`levelUpOrange_gte.95`, `levelUpNotOrange_lte.05`, `levelUpNotOrange2_lte.05`). ⚠ The two `levelUpNotOrange` guards' 4K region overrides are broken — see Integration issues.

### 8. Heat / Sun  ·  `sunYellow`, `sunNotYellow`, `sunRed`, `sunNotRed`

The sun/heat warning appears center-screen (the small sun icon below the compass). `sunYellow` ≥ .95 (guard `sunNotYellow` ≤ .05) → **Heat Warning**; `sunRed` ≥ .95 (guard `sunNotRed` ≤ .05) → **Heatstroke**.

Heat warning (sun icon yellow):

![heat warning](images/heatWarning.png)

Heatstroke (sun icon red):

![heatstroke](images/heatstroke.png)

*Reference:* `…/sunYellow/sunYellow_gte.95.png`, `…/sunNotYellow/sunNotYellow_lte.05.png`, `…/sunRed/sunRed_gte.95.png`, `…/sunNotRed/sunNotRed_lte.05.png`

### 9. Worm  ·  `wormRed`, `wormNotRed`

When a sandworm is near, a warning indicator appears center-screen. `wormRed` ≥ .95 with `wormNotRed` ≤ .05 fires the **Worm** effect.

![worm](images/worm.png)

*Reference:* `…/wormRed/wormRed_gte.95.png`, `…/wormNotRed/wormNotRed_lte.05.png`

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| Top-center zone indicator (gate) | `inGameYellow1`, `inGameYellow2`, `inGameGreen`, `inGameRed`, `inGameNotYellow` |
| Bottom-left HUD — health + shield | `healthCheckRed`, `healthCheckNotRed`, `healthBarRed`, `shieldCyan` |
| Bottom-left HUD — energy | `energyCheckYellow`, `energyCheckNotYellow`, `energyBarYellow` |
| Bottom-left HUD — vitality | `vitalCheckGreen`, `vitalCheckNotGreen`, `vitalBarGreen` |
| Far-left water/shelter icon | `waterBlue`, `waterRed` |
| Bottom-right vehicle module | `rideYellow`, `rideNotYellow`, `rideBlack`, `rideNotBlack` |
| Center level-up banner | `levelUpOrange`, `levelUpNotOrange`, `levelUpNotOrange2` |
| Center heat/sun warning | `sunYellow`, `sunNotYellow`, `sunRed`, `sunNotRed` |
| Center worm warning | `wormRed`, `wormNotRed` |

**30 meters across 9 screens** (the bottom-left HUD groups health, shield, energy and vitality meters into essentially one frame).

---

## Notes

* **In-game gate is the top-center zone indicator** — its color also selects the Zone effect (yellow PVE / green safe / red PVP). Every effect requires the gate.
* **Check + Bar + guard pattern** — health, energy and vitality each use a *Check* meter (bar present), a *Bar* meter (fill level), and a *Not* guard. Low-X fires when the bar ≤ .25; the HUD mirrors the bar while present.
* **Paired state effects** — Shield (up/down), Water (sheltered/exposed) and Heat (warning/heatstroke) are two-sided; capture both colors.

### ⚠ Integration issues found in `DuneAwakening.html` (worth fixing before QA)

1. **Exposed never fires** — `WaterRedMeter.setValue(engine.vision.vitalRed)` reads `vitalRed`, which is **not a defined meter**. It should read `engine.vision.waterRed`. As written, the Water "Exposed" effect can never trigger.
2. **Vehicle broken at 3840x2160** — the `rideYellow` and `rideNotYellow` meters' `3840x2160` overrides point to `x=0.4979 y=0.0472` (the top-center compass region) instead of the bottom-right vehicle module, so the Vehicle effect won't detect at 4K.
3. **Level-up guards broken at 3840x2160** — the `levelUpNotOrange` and `levelUpNotOrange2` meters' `3840x2160` overrides point to `x=0.9677 y=0.8101` (the vehicle area) instead of the level-up banner, which will misread the guards at 4K.

* **Capture status** — 22 of 30 meters captured at **3840x2160 SDR**, organized in `DuneAwakening/DuneAwakening/3840x2160/SDR/<meter>/`. Missing: `inGameRed`, `ride*` (4), `levelUp*` (3). Other resolutions / HDR not captured yet.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

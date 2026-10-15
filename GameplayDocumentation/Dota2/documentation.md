# Dota 2

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

The integration also carries aspect-ratio fallbacks (**3.55:1**, **24:10** and **16:11**), so other resolutions sharing those aspect ratios are read from the nearest matching profile.


---

## Resolution Setup

Change the monitor resolution, launch the game and open **Settings → Video**.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Display Mode** so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Health effect**, **Mana effect**, **Death**, **Low Health**, **Level Up**, **Victory/Defeat**, **Teleport**, **Night/Day**, **Searching Match** and **Courier**. The Health and Mana HUD bars have color, position and size controls, and an **Adjustment Toggle** (turn it off in game) shows both bars at full size so they can be placed on the keyboard.

Most effects only fire while the integration considers you **in game** (see step 1). The exceptions are **Searching Match**, which reads the main menu, and **Victory**, which is read on the end-of-match screen.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

### 1. In game (gate)  ·  `Health`, `Mana`, `PlayingHero`

The in-game state turns on when the **health bar** (`Health` ≥ .1), the **mana bar** (`Mana` ≥ .1) and the **hero portrait** (`PlayingHero` ≥ .0001) all read at the bottom of the screen. Health, Mana and PlayingHero use the same screenshot. **Most effects require this state.**

Test with a full bar (`Health_gte.1`, `Mana_gte.1`) and the hero portrait visible (`PlayingHero_gte.0001`).

![in game](images/in_game.png)

### 2. Hover block  ·  `HoverBlock`

A small region at the right end of the health bar. `HoverBlock` ≥ .9 means it reads normally; when it drops (for example with the mouse over the bar) the HUD bar is held, and the health effects only run once it has read clear for 2 seconds.

![hover block](images/hover_block.png)

### 3. Death  ·  `Death`

The meter reads a small region of the **health bar text** at the bottom of the screen, which changes when you die (the bar empties and the BUYBACK panel appears): `Death` increases by more than .1 (to ≥ .1). Plays the **Death** effect (the screen fades to black, then blood splatters from the left and right edges). Only read during a match, and for 10 seconds after leaving it.

![death](images/death.png)

### 4. Level Up  ·  `LevelUp`

A thin region at the very bottom of the screen, just left of the hero level badge. It is change-based: the effect fires when the meter *decreases* by more than .1 down to ≤ .01 (on level up). Confirm the exact on-screen cue live. Plays the **Level Up** effect (white and yellow arrows rising across the screen for 5 seconds).

![level up](images/level_up.png)

### 5. Victory / Defeat  ·  `VictoryDefeatConfirm`, `RadiantVictory`, `DireVictory`

The **Victory** screen: `VictoryDefeatConfirm` ≥ .95 (the continue button) plus the team banner — `RadiantVictory` ≥ .95 or `DireVictory` ≥ .95. Plays the **Victory** effect, in green for Radiant or red for Dire (a radial burst with falling particles for about 8 seconds). `VictoryDefeatConfirm` and `DireVictory` share one screenshot; `RadiantVictory` needs its own.

Dire victory:

![dire victory](images/dire_victory.png)

Radiant victory:

![radiant victory](images/radiant_victory.png)

### 6. Night / Day  ·  `NightDay`

The **day/night clock** at the top center (a sun or moon icon). `NightDay` is a color-mean meter: a value between 190 and 210 means the icon flipped to night, and between 30 and 55 means it flipped to day. It fires on the **transition** only. Plays the **Night/Day** effect (the screen fades to a dark blue or light blue while a sun and moon orbit, about 5 seconds).

![night day](images/night_day.png)

### 7. Teleport  ·  `TeleportBlack`, `TeleportYellow`, `TeleportBlue`

Using a **Teleportation Scroll** shows the channeling bar in the center of the screen. All three meters must read at once (≥ .95): the dark bar background, the yellow scroll icon and the blue progress fill. Plays the **Teleport** effect (a cyan spiral with an expanding wave). The three meters share one screenshot.

![teleport](images/teleport.png)

### 8. Courier  ·  `CourierCircle`

A small region at the bottom left, around the **courier button** (F2) next to the shop. It is change-based: the courier **starts a delivery** when `CourierCircle` drops below .1 (a drop of more than .5), and **finishes** when it reads ≥ .95 again. Plays the **Courier Start** effect (a running horse for 7 seconds) and the **Courier Finish** effect (a box drops in with yellow and blue particles).

![courier](images/courier.png)

### 9. Searching Match  ·  `searchingMatch`

The **FINDING MATCH** banner in the bottom right of the main menu: `searchingMatch` ≥ .95. Only read **out of game**. A lingering effect (animated clouds on a sky-blue background) that holds while the banner is visible and fades out when it disappears (`searchingMatch` < .1).

![searching match](images/searching_match.png)

### 10. Radar and Tower  ·  `radarBlue`, `towerYellow`  *(disabled)*

The **Radar** (scan) and **Tower enforcement** effects are written but **switched off** in the script (`enableRadar` and `enableTower` are hard-coded to `false`, and their UI toggles are commented out). The meters and test images exist so they can be validated later. Both would fire when their icon fades (meter < .1, a drop of more than .5).

Radar:

![radar](images/radar.png)

Tower:

![tower](images/tower.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (HP / mana bars + hero portrait) | `Health`, `Mana`, `PlayingHero` |
| Hover block (mouse over the HUD) | `HoverBlock` |
| Death (buyback panel) | `Death` |
| Level Up indicator | `LevelUp` |
| Victory screen | `VictoryDefeatConfirm`, `RadiantVictory`, `DireVictory` |
| Day / night clock | `NightDay` |
| Teleport channeling bar | `TeleportBlack`, `TeleportYellow`, `TeleportBlue` |
| Courier icon | `CourierCircle` |
| FINDING MATCH banner (menu) | `searchingMatch` |
| Radar icon *(disabled)* | `radarBlue` |
| Tower icon *(disabled)* | `towerYellow` |

**17 meters across 11 screens.**

---

## Notes

* **Health and Mana effects are not wired up** — `Healthcallback()` (damage splatter, healing orbs, Low Health) and `Manacallback()` (Low Mana) are defined in `Dota2.html`, but nothing calls them: `Health_Meter` and `Mana_Meter` are created with an empty callback (`() => ""`). Only the HUD bars follow the meters; the damage, healing, Low Health and Low Mana effects will **not** fire until the callbacks are connected. Verify this live and either wire the callbacks to the meters or remove the unused toggles.
* **In-game detection gates most effects** — Level Up, Teleport, Courier and Night/Day all require the in-game state. **Searching Match** reads outside it, and **Victory** is read on the end screen.
* **Victory and Death reading window** — `Death` and the Victory meters are only read while in a match and for 10 seconds after the hero portrait was last seen, so the end screen is still detected.
* **Effects pause the others** — the Night/Day and Death effects block meter reads while they are playing (`dayNightPlaying`, `deathPlaying`), so trigger them deliberately rather than mid-sequence.
* **Change-based meters** — Death, Level Up, Courier and Night/Day fire on the *change* of the meter. Validate them **live** and not just from a static frame.
* **Teleport needs all three meters** — `TeleportBlack`, `TeleportYellow` and `TeleportBlue` must all read ≥ .95 at the same time.
* **Staging file** — `DotaStaging.html` is an older staging version (it has an Effect Choice / Replay Effect preview) and is not the release file.
* **Capture status** — 17 test images captured at 3840x2160 SDR only (`Health`, `Mana` and `PlayingHero`, the three Teleport meters, and `VictoryDefeatConfirm` / `DireVictory` are identical files). See `meters.txt`. Other resolutions and HDR are not captured yet.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

# AION 2

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

Change the monitor resolution, launch the game and open the display/graphics settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display mode so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Action effect**, **Gather effect**, **Fly effect**, **Quest effect**, **Fight effect**, **Death effect**, **Damage effect**, **Low health effect**, **Low mana effect** and **Travel effect**. The Health and Mana HUD bars have on/off, color, position and size controls, and a **Bar Effect Adjust** toggle (turn it off in game) shows both bars at full size so they can be placed on the keyboard.

Most effects only fire while the integration considers you **in game** (see step 1). The exceptions are **Death**, which fires while the in-game state is *off*, and **Quest**, which does not check it.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image - you do **not** need one screenshot per meter.

Test images live in `AION_2/<resolution>/<SDR|HDR>/<meter>/<meter>_<op><value>.png` (see `meters.txt` for the exact filenames).

### 1. In game (gate)  ·  `inGameBlack`, `inGameWhite`, `inGameNotWhite`

The in-game state turns on when the HUD marker under the level badge at the bottom center of the screen reads: `inGameBlack` >= .9 **and** `inGameWhite` >= .4 with `inGameNotWhite` < .1. All three meters use the same screenshot. **Most effects require this state.**

![in game](images/in_game.png)

### 2. Action  ·  `actionBlue`, `actionNotBlue`

The blue "hold to interact" bar appears (search, heal, ...): `actionBlue` >= .5 with `actionNotBlue` < .1. Plays the **Action** effect (a progress ring fills clockwise, then a flash and shockwave on completion, about 3 seconds).

![action](images/action.png)

### 3. Fly  ·  `flyBlueBar`, `flyNotBlue`

The blue flight bar appears: `flyBlueBar` >= .3 with `flyNotBlue` < .1. Plays the **Fly** effect (soft clouds rush outward from the horizon while a pair of wings flaps at the bottom). Loops while airborne.

![fly](images/fly.png)

### 4. Travel  ·  `travelBlue1`, `travelBlue2`, `travelNotBlue`

Blue boxes on the auto-path indicator next to the quest tracker: `travelBlue1` >= .95 **and** `travelBlue2` >= .5 with `travelNotBlue` < .1. Plays the **Travel** effect (white dots rise from the horizon and sweep past along a winding route, drifting cyan to violet). Loops while auto-pathing.

![travel](images/travel.png)

### 5. Fight  ·  `fightRed`, `fightTextYellow`, `fightNotRed`

Enemy/boss red HP bar + yellow name at the top of the screen: `fightRed` >= .95 **and** `fightTextYellow` >= .2 with `fightNotRed` < .1. Plays the **Fight** effect (two blades slam in from the sides and clash at the center with a white flash, sparks and a screen shake, about 2 seconds).

![fight](images/fight.png)

### 6. Quest complete  ·  `questWhite`, `questTextWhite`, `questNotWhite`

White star + white text on the quest banner: `questWhite` >= .95 **and** `questTextWhite` between .1 and .3 with `questNotWhite` < .1. Plays the **Quest** effect (a green check draws itself in with gold ring pulses and drifting sparkles, about 3.5 seconds). **Does not require the in-game state.**

![quest complete](images/quest_complete.png)

### 7. Gather  ·  `gatherGreenBar`, `gatherSucessBlue`, `gatherFailOrange`

The green gather bar is up at the top of the screen (`gatherGreenBar` >= .95). Plays the **Gather** effect: blue fills the keyboard from the left by the **Success** bar's value and orange fills from the right by the **Fail** bar's value, split by a white line where they meet. When gathering ends, the winning color floods the keyboard and fades out. `gatherSucessBlue` and `gatherFailOrange` only drive the fills and have no threshold of their own. All three meters use the same screenshot.

![gather](images/gather.png)

### 8. Death  ·  `deathTextWhite`, `deathTextWhite2`, `deathNotWhite`

White text on the defeat screen: `deathTextWhite` and `deathTextWhite2` both between .2 and .5 with `deathNotWhite` < .1. Plays the **Death** effect (red drains to grey, a vignette closes in, soul wisps rise, then black, about 7 seconds). **Fires while the in-game state is OFF.**

![death](images/death.png)

### 9. Health  ·  `healthRedBar`

HP bar fill (0..1), left of the level badge. Drives the Health HUD bar. A drop of >= 3% fires **Damage** (red flash with three claw slashes); <= 30% fires **Low Health** (red heartbeat vignette that loops until health goes back above 35%).

![health](images/health.png)

### 10. Mana  ·  `manaBlueBar`

Mana bar fill (0..1), right of the level badge. Drives the Mana HUD bar. <= 20% fires **Low Mana** (blue heartbeat vignette that loops until mana goes back above 35%).

![mana](images/mana.png)

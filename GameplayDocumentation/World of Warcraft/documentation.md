# World of Warcraft

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

The integration targets WoW's **default UI**. Addons that move or reskin the minimap, player frame or XP bar will break detection.

---

## Resolution Setup

Change the monitor resolution, launch the game and open **Options → Graphics**.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Display Mode** so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

The effects have on/off toggles in the SignalRGB UI: **Level Up Effect**, **Death Effect**, **Combat Effect** and **Aura Effects** (Aura Effects covers both the Screen Aura and the Personal Aura). The Health and Mana HUD bars have on/off, color, position and size controls, and a **HUD Adjust Toggle** (turn it off in game) draws both bars so they can be placed on the keyboard.

Every effect except the auras only fires while the integration considers you **in game** (see step 1).

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image - you do **not** need one screenshot per meter.

Test images live in `World of Warcraft/<resolution>/<SDR|HDR>/<meter>/<meter>_<op><value>.png` (see `meters.txt` for the exact filenames).

### 1. In game (gate)  ·  `mapYellow`, `mapWhite`, `notMapYellow`

Load into the world with the default HUD visible. The **minimap** in the top-right corner sets the in-game state: the gold border (`mapYellow` >= .95) **and** the white highlight (`mapWhite` >= .95) with `notMapYellow` <= .05 beside it. The state clears when `mapWhite` <= .25 **and** `mapYellow` <= .05 (loading screens, menus). **Most effects require this state.**

The same screenshot also covers the **player frame** bars below the character: `healthGreen` (green health bar) and `manaBlue` (blue mana bar). Once either bar reads >= .1 in game, its HUD bar turns on.

![in game](images/in_game.png)

### 2. Combat  ·  `combatRed`

Attack an enemy. The **player frame portrait** gets a red combat ring: `combatRed` >= .95. Plays the **Combat** effect (two red swords rise from the bottom and cross over the center, a yellow ring expands outward, then they fade, about 2 seconds). It re-arms once the effect ends.

![combat](images/combat.png)

### 3. Level Up  ·  `xpPurple`

The purple **XP bar** at the bottom of the screen. `xpPurple` >= .95 (bar almost full) arms the effect, and when the bar wraps back to empty on level-up (`xpPurple` <= .05) it plays the **Level Up** effect (yellow orbs spiral out from the center, three full turns, about 7 seconds). The XP bar is visible in the Combat screenshot above.

Level Up is a **transition** effect, so validate it **live** by levelling up rather than from a static frame.

### 4. Death  ·  `deathBlack`, `healthGreen`

Let an enemy kill you. The black **"Release Spirit"** box appears at the top-center: `deathBlack` >= .95 **and** the health bar is empty (`healthGreen` <= .05). Plays the **Death** effect (a dark teal overlay fades in, then a pale blue winged spirit rises off the top of the screen, about 6.5 seconds). The minimap is still visible, so the in-game state stays on.

![death](images/death.png)

### 5. Auras  ·  `outerVibe1`, `outerVibe2`, `innerVibe1`, `innerVibe2`

These are **colormean** meters - they return a color, not a value, so they have no threshold. `outerVibe1` / `outerVibe2` sample the outer edges of the screen and drive the **Screen Aura** (a soft ring of light around the keyboard edges). `innerVibe1` / `innerVibe2` sample the area around your character and drive the **Personal Aura** (a small glowing ring at the center). Both auras are lingering: they follow the scene colors while you play and fade out when you leave the game. Move between zones with different lighting to see the colors change.

![aura](images/aura.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (minimap) | `mapYellow`, `mapWhite`, `notMapYellow` |
| Player frame bars (HUD) | `healthGreen`, `manaBlue` |
| Combat (portrait ring) | `combatRed` |
| Level Up (XP bar) | `xpPurple` |
| Death (Release Spirit box) | `deathBlack` + `healthGreen` |
| Auras (scene colors) | `outerVibe1`, `outerVibe2`, `innerVibe1`, `innerVibe2` |

**12 meters across 4 screenshots** driving 5 effects + 2 HUD bars.

---

## Notes

* **The in-game gate is the minimap.** If nothing fires, first confirm the minimap border/highlight is being read (step 1). Hiding the minimap or using a minimap addon turns every effect off.
* **The aura push does not check the gate.** Auras can start out of game, then fade out right away, which may flicker on menus and loading screens. Validate the auras on menus as well as in game.
* **Health/Mana HUD bars** - the bar width is scaled too far in the current build (it looks full until HP drops below about 31%) and it is not drawn where the **HUD Adjust Toggle** preview shows it. Check the bars live while taking damage and spending mana.
* **Capture status** - only 3840x2160 SDR has been captured so far (12 images). See `meters.txt` and `TO_CAPTURE.txt` for the 84 images still to take.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** - the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** - the lighting effect actually triggers and looks correct.

# Diablo IV

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

The integration also carries aspect-ratio fallbacks (**3.55:1**, **24:10** and **16:11**), so other resolutions sharing those aspect ratios are read from the nearest matching profile. **2560x1080** and **3840x2160** also have their own dedicated profiles. HDR color ranges are defined for the main meters.

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video/graphics settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Display Mode** / aspect ratio so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Health Warning Effect**, **Mana Warning Effect**, **Death Effect**, **Safe Zone Effects**, **Ability Effects**, **Dodge Effect**, **Flask Effect**, **Level Up Effect** (also covers the skill point effect), **Event Completion Effect**, **Quest Completion Effect**, **Buff Effect**, **Waypoint Discovery Effect** and **In-Menu Effect**. **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

The **Health Bar** and **Mana Bar** HUD overlays each have on/off, color, X/Y position and size controls, plus a **HUD Background Black** option. Turn on **Adjustment Mode** to show both bars at full width so they can be placed on the keyboard; it is ignored while in game, but turn it off before playing.

Most effects only fire while the integration considers you **in game** (see step 1). The exceptions are **Death** and the **In-Menu** fire, which run while the in-game state is *off*.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

Test images live in `SDR/<meter>/<meter>_<op><value>.png` (the screenshots were captured at 2560x1440 SDR).

### 1. In game (gate)  ·  `inGame`, `inGame2`

The in-game state turns on when two HUD landmarks on the action bar read at the same time: `inGame` >= .95 (left of the skill bar) **and** `inGame2` >= .95 (right of the skill bar). It is also off whenever the death screen is up (both death meters at 1) or health is nearly empty (`health` <= .05). **Most effects require this state.**

![in game](images/in_game.png)

### 2. Health  ·  `health`

The red health orb at the bottom-left: `health` is the filled portion of the orb (0..1). It drives the **Health Bar** HUD. Below 20% it fires the **Low Health** effect (a pulsing red vignette with blood drops falling), and it stops once health is back above 25%.

![health](images/health.png)

### 3. Mana  ·  `mana1`, `mana2`

The blue resource orb at the bottom-right, read in two vertical strips (`mana1` and `mana2`). The average drives the **Mana Bar** HUD. Below 20% (`mana1` < .2) it fires the **Low Mana** effect (a pulsing vignette in the mana color), and it stops once mana is back above 25%.

![mana](images/mana.png)

### 4. Flask  ·  `flaskWhite`

The potion icon above the health orb lights up white when a flask is drunk: `flaskWhite` >= .95. Plays the **Flask** effect (green bubbles rise from the bottom of the keyboard). The icon is visible at the top-left of the health orb in the in-game screenshot above.

### 5. Dodge  ·  `dodge`

The dodge icon next to the mana orb lights up when you dodge: `dodge` > .95. Plays the **Dodge** effect (an orange streak sweeps across the keyboard).

![dodge](images/dodge.png)

### 6. Abilities  ·  `ability1` … `ability6`

The six skill slots on the action bar. Each slot is read as a color: the **lightness** drop when the skill goes on cooldown fires the effect (a drop of more than 5), and the **hue** of the icon sets the particle color. Plays the **Ability** effect (a burst of particles in the skill's color). Each slot has a 300 ms cooldown.

Ability use is change-based, so validate it **live** by using each skill rather than from a static frame. All six meters use the same screenshot.

![ability](images/ability.png)

### 7. Level Up  ·  `level`

The XP bar at the bottom of the action bar, next to the level badge: `level` reads its filled start, and when it **empties** (drops to <= .1) the level was gained. Plays the **Level Up** effect (a yellow ring expands from the center while arrows rise).

Level up is change-based, so validate it **live**. The level-up banner is visible in the health screenshot above (`LEVEL 12`).

### 8. Skill point available  ·  `skillPointGrey`, `skillPointYellow`

The skill-point diamond at the right of the mana orb: the grey frame (`skillPointGrey` >= .95) **and** the yellow diamond (`skillPointYellow` >= .95). Plays the **Skill Point** effect (yellow rings repeatedly expand) for as long as a point is available.

![skill point](images/skill_point.png)

### 9. Safe zone  ·  `safeZoneAbility0`

The skill bar dims and loses its saturation when you step into a town or other safe zone: `safeZoneAbility0` jumps by more than .4 to above .5. Plays the **Enter Safe Zone** effect (a ring expands in warm to cool colors), and when it drops back below .2 the **Exit Safe Zone** effect plays in reverse.

This is change-based, so validate it **live** by walking in and out of town.

![safe zone](images/health.png)

### 10. Death  ·  `deadRed`, `deadBlack`

The **YOU HAVE DIED** screen: the red banner emblem (`deadRed` >= .95) and the dark band (`deadBlack` >= .95). **Fires while the in-game state is OFF.** Plays the **Death** effect (a pale orb grows from the center while red particles orbit it, about 10 seconds).

![death](images/death.png)

### 11. Event complete  ·  `eventCompleteWhite`, `eventCompleteRed`, `eventCompleteRed2`

The **EVENT COMPLETE** banner at the top-center: the white title text (`eventCompleteWhite` >= .6) and the red emblem under it (`eventCompleteRed` + `eventCompleteRed2` >= .75). Plays the **Event Complete** effect (the keyboard darkens while embers and sparks rise). It re-arms once the banner is gone.

The two red meters read the same region; their values are added together.

![event complete](images/ability.png)

### 12. Quest complete  ·  `questColor`, `questRed`, `questWhite`

The **QUEST COMPLETE** banner at the left of the screen: the red ribbon (`questRed` > .3) and the white title text (`questWhite` > .1), with `questColor` reading the hue of the quest emblem. Plays the **Quest Complete** effect (a series of V-shaped waves rise in the quest color). It re-arms once the banner is gone.

![quest complete](images/quest_complete.png)

Second quest banner (the red ribbon and text meters):

![quest complete 2](images/quest_complete_2.png)

### 13. Buff  ·  `buffedGreen`, `buffedNotGreen`

The green-bordered buff icon above the action bar appears: `buffedGreen` > .3 and rising, with `buffedNotGreen` <= .1. Plays the **Buff** effect (green waves rise from the bottom of the keyboard). The buff icon is visible just above the skill bar in the health screenshot (step 2). It re-arms once the icon is gone.

### 14. Waypoint discovered  ·  `waypointBlue`, `waypointRed`, `waypointRed2`

The **WAYPOINT UNLOCKED** banner at the lower center: the blue triangle (`waypointBlue` >= .95) inside the red emblem (`waypointRed` + `waypointRed2` >= .95). Plays the **Waypoint** effect (colored particles spiral in toward the center).

![waypoint](images/waypoint.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (action bar) | `inGame`, `inGame2` |
| Health orb | `health` |
| Mana orb | `mana1`, `mana2` |
| Flask | `flaskWhite` |
| Dodge | `dodge` |
| Skill slots | `ability1`, `ability2`, `ability3`, `ability4`, `ability5`, `ability6` |
| XP bar (level up) | `level` |
| Skill point icon | `skillPointGrey`, `skillPointYellow` |
| Safe zone (skill bar dim) | `safeZoneAbility0` |
| Death screen | `deadRed`, `deadBlack` |
| Event complete banner | `eventCompleteWhite`, `eventCompleteRed`, `eventCompleteRed2` |
| Quest complete banner | `questColor`, `questRed`, `questWhite` |
| Buff icon | `buffedGreen`, `buffedNotGreen` |
| Waypoint banner | `waypointBlue`, `waypointRed`, `waypointRed2` |

**30 meters across 14 screens** driving 11 effects (plus the Health and Mana HUD bars and the In-Menu fire).

---

## Notes

* **In-game detection gates most effects.** Death only fires while the gate is OFF, and the **In-Menu** fire (embers over a red glow) runs whenever you are out of game, until a quest or event effect takes over.
* **Several states are change-based** — abilities (lightness drop), level up (XP bar empties), buff (value rising) and safe zone (jump in the skill bar) trigger on the transition, so validate them **live** rather than from a static frame.
* **Positive / negative pairs** — several states use a positive meter plus a `…NotGreen` negative guard (e.g. `buffedGreen` + `buffedNotGreen`). Both must read correctly for the effect to fire cleanly.
* **Summed meters** — the `…Red` and `…Red2` pairs (event complete, waypoint) read the same region with slightly different ranges; their values are added.
* **Shared screenshots** — all six ability meters, both event-complete red meters, and the quest, waypoint and gate meters each share one image.
* **Unused meters** — the file also defines placeholder meters for mounts, teleport and combat (`mountWhite`, `mountNotWhite`, `greenBar`, `greenConfirmation`, `combatRed`, `combatBlack`). They are not read by any effect yet, so they are not part of this test.
* **Legacy test images** — the `SDR` folder also holds images for older meters (`mana`, `manaBright`, `manaCol`, `vigor`, `vigorBright`, `safeZoneAbility1`, `health2`, `buffedBlack`). The current integration reads `mana1` / `mana2` instead; the old mana orb screenshot is used above.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

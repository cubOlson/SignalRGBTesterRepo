# World of Tanks

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

The integration also carries aspect-ratio fallbacks, so other resolutions sharing those aspect ratios are read from the nearest matching profile.

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the Graphics / Window Size option, then set the game to the resolution being tested. If it is not available, adjust the Window Mode setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

**Keep the battle HUD on its default layout.** The meters read fixed regions: the player panel in the lower-left, the team score bar at the top, the minimap in the lower-right, the damage panel in the lower-center and the battle messages in the center. Do not move, hide or rescale the minimap or the panels.

---

## Effect Controls

The effects in the SignalRGB UI are driven by the battle HUD. The controls are:

* **HUD effects** — turns the Health HUD bar on or off.
* **Health HUD Color**, **Health Bar X / Y / Width / Height** — the color, position and size of the Health HUD bar that fills the devices as you lose health.
* **HUD Adjust Toggle (turn off in-game)** — draws the Health HUD bar at full size while you are out of the game, so you can position it before testing. **Turn it off before you test in a battle.**
* **Custom HUD Background** — gives the Health HUD bar its own background color.
* **Background Mode** — sets the backdrop. It can be Ambience (mirrors the screen), Color Cycle, Custom, or any of the Audio Visualizer variants.
* **Background Brightness** and **Custom Color** — the brightness and solid color used by the backdrop.

Every effect below only fires while the integration considers you **in game** (see step 1).

---

## A Note on Testing World of Tanks

Battles last about 10 to 15 minutes, and some states only appear at the end. To test efficiently:

* **Play a normal Standard Battle** (random battle). Most of the meters appear naturally while you fight.
* **A training room or a short battle** also works, because you can get destroyed or finish quickly.
* **Order matters for some states.** Death must come before the Replay, and the Victory / Defeat banner can only be captured when a battle actually ends, so the win and loss tests may need two battles.

**One screen can drive several meters**, so you do not need a separate capture per meter. A single image of the damage panel, for example, validates every damage meter at once.

> **Before you queue**, make sure the **battle minimap** and the **player panel** are visible in the HUD settings.

---

## Testing Guidelines

### 1. Enter a battle — in game

Load into a battle. As soon as the HUD is visible, the in-game state activates and the other effects unlock.

* **In-game detection** — `inGameBrown1` and `inGameBrown2` (the dark HUD panels) must read high while `inGameNotBrown` stays low. **Every effect below requires this state.**
* **Health HUD** — the `healthBar` meter reads the health bar in the lower-left player panel, and the Health HUD bar on the devices follows it.

![ingame](images/ingame.png)

### 2. Get spotted — discovered

Let an enemy spot you, or spot one yourself. The yellow **light bulb** icon appears at the top-center of the screen. `discoveredYellow` triggers a tank-with-ripple animation. It does not fire while the Death effect is active.

![discovered](images/discovered.png)

### 3. Enemy on the minimap

Get an enemy to show on the minimap (spotted by you or by a teammate). `enemyMapRed` reads the red enemy markers on the minimap. When any red appears (≥ 0.005), a radar effect plays for about 6 seconds, and it resets once the markers have disappeared (< 0.003).

![enemy map](images/enemy_map.png)

### 4. Hit an enemy

Fire at an enemy and hit them. The damage panel shows a green **Damage Caused** line, which triggers `hitSuccessfulGreen`. It must appear together with a white cue (`damageWhite` or `criticalDamageWhite`) to play the lime X effect.

![hit](images/hit.png)

### 5. Take damage

Let an enemy hit you. The red **Damage by Shot** line in the lower-center triggers `damageRed`, together with `damageWhite`, and plays the red X effect.

![damage](images/damage.png)

### 6. Critical damage

Take a hit that damages a module or injures a crew member (for example *engine damaged*). The orange **Critical Damage** line triggers `criticalDamageYellow` together with `criticalDamageWhite`, and plays the orange gear effect.

![critical](images/critical.png)

### 7. Low health

Take enough damage that the health bar is mostly empty. When `healthBar` reads **0.7** or higher, a pulsing red glow plays at the bottom of the devices. It stops when you die or leave the battle.

![low health](images/low_health.png)

### 8. Team score

During the battle, the score box at the top highlights whichever team scores (a kill). A green highlight on your side triggers `scoreGreenWin` and a tank with a green arrow moves left to right. A red highlight on the enemy side triggers `scoreRedWin` and a tank with a red arrow moves right to left.

| Green (your team) | Red (enemy team) |
| --- | --- |
| ![score green](images/score_green.png) | ![score red](images/score_red.png) |

### 9. Die

Let the enemy destroy your tank. The orange **Vehicle destroyed** message in the center reads `deadOrange`, guarded by `deadNotOrange`, and plays the tank explosion. While Death is active the Health HUD hides, and the other effects (discovered, damage, critical, hit) are blocked.

![dead](images/dead.png)

### 10. Replay

After you are destroyed, the game can play a **cinematic replay** of the kill with the **ESC Skip** and **SPACE Pause** prompts. `replayOrange` reads the orange prompts (guarded by `replayNotOrange`) and ends the Death state. Skip the replay with **ESC** to finish.

![replay](images/replay.png)

### 11. Victory or defeat

Finish a battle. The result banner is read by three meters together:

* **Victory** — the blue **VICTORY!** banner: `victoryBlue`, plus the white sub-text `victoryDefeatWhite` and `victoryDefeatWhite2`. The devices show a tank with green fireworks.
* **Defeat** — the red **DEFEAT** banner: `defeatRed`, plus the same two white meters. The devices show a tank with red fireworks.

You can capture either one after the last enemy tank or the base capture finishes the battle. You can still be alive or dead when the banner shows.

| Victory | Defeat |
| --- | --- |
| ![victory](images/victory.png) | ![defeat](images/defeat.png) |

---

## Notes

* **In-game detection gates every effect** — the in-game state (step 1) has to be active, or none of the effects fire. If nothing triggers, check the HUD layout first.
* **Death has priority** — while it is active it hides the Health HUD and blocks discovered, damage, critical and hit. The Replay screen then ends it.
* **Paired meters** — most states use a main meter plus a guard or a second confirmation meter (for example `damageRed` + `damageWhite`, `deadOrange` + `deadNotOrange`, or `victoryBlue` + `victoryDefeatWhite` + `victoryDefeatWhite2`). All of them must read correctly for the effect to fire.
* **Damage panel shares one region** — `damageRed`, `criticalDamageYellow` and `hitSuccessfulGreen` all read the same lower-center region, but with different colors. Trigger them with separate hits.
* **Screenshot frames in this folder** — the QA screenshots in `SDR/` are organized per meter, and the number at the end of each file name is the match threshold used by the meter (for example `victoryBlue_gte.8` = at least 0.8, `deadNotOrange_lt.1` = below 0.1). Only 3840x2160 SDR frames are included, so other resolutions still need their own captures.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

# Hollow Knight: Silksong

## Supported Resolutions

* 1920x1080
* 1920x1200
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 5120x1440
* 3840x2160 (HDR)

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the Resolution option, then set the game to the resolution being tested. If it is not available, adjust the Aspect Ratio setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Testing Guidelines

Observation: Most effects only fire while the integration considers you "in game" (the gameplay HUD is visible, not a menu/cutscene). Load into gameplay first, then trigger the individual states below.

1. Load into normal gameplay so the HUD is visible. This activates the in-game detection, triggering the `inGameWhite`, `inGameBrown` and `inGameNotWhite` meters.
2. With the HUD visible, the mask (health) bar in the top-left is read by the `healthWhite` meter. Take a hit so a mask is lost — the sharp drop triggers the Damage effect.
![inGame](images/inGame.png)

3. Keep taking damage until you are down to roughly one or two masks (low health). This triggers the Low Health effect off the `healthWhite` meter.
![lowHealth](images/lowHealth.png)

4. Build up your silk gauge (attack enemies) so the silk bar fills. The silk bar is read by the `silkWhite` meter, and a full gauge triggers the Full Silk effect off the `ultWhite` meter.
![silk](images/silk.png)

5. Spend a full silk gauge to Bind / heal. The sharp drop in silk triggers the Bind effect off the `ultWhite` meter.
![bind](images/bind.png)

6. Sit at a bench to Rest. This triggers the `rest1White` and `rest2White` meters.
![rest](images/rest.png)

7. Collect an item / pickup (a bright popup on screen). This triggers the `itemColor`, `itemsWhite` and `itemsNotWhite` meters.
![item](images/item.png)

8. Enter a Boss fight so the boss health bar appears at the bottom of the screen. This triggers the `bossWhite` and `bossNotWhite` meters.
![boss](images/boss.png)

9. Trigger a dialogue box (talk to an NPC). The dialogue box triggers the `dialogueTopWHite` and `dialogueBottowWHite` meters.
![dialogue](images/dialogue.png)

---

## Notes

* Most effects are gated by the in-game detection — if the HUD is hidden (menus, cutscenes, map), health, silk, item and boss effects will not fire.
* The Health Bar and Silk Bar HUD readers each have their own enable toggle and X/Y/width/height controls — if a bar is misread, check that the HUD overlay is aligned to the on-screen bar and turn off the Bar Adjust Toggle before testing.
* Effect groups can be toggled independently in the integration (Damage, Low Health, Full Silk, Rest, Bind, Boss, Item, Dialogue). Make sure the relevant effect is enabled before testing its meter.
* Meter validation alone does not guarantee the integration is working. A complete test should verify both:
  * Meter validation (correct region and color detection)
  * Effect validation (the lighting effect actually triggers and looks correct)

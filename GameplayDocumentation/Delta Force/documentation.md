# Delta Force

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 5120x1440

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Screen resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every event below has its own on/off toggle in the SignalRGB UI (Kill, Dead, Downed, Extract, Failed Extract, Low Ammo, Round Start, Task, Inventory, Low Health, Stamina, Action).

The **Health Bar Effect** is a persistent HUD bar with its own color, position and size controls (Health HUD Color, Health Bar X / Y / Width / Height). Use **Bar Effect Adjust** to draw the health bar at full size while out of game so you can position it before testing.

---

## Testing Guidelines

Most effects only fire while the integration considers you **in game** — the white lettering in the compass at the top-center of the screen is used as the "playing" gate (`inGameWhite` positive, `inGameNotWhite` negative, `inGameHealth` confirming the health bar is on screen). Enter a match first, then trigger the individual states below. Follow the steps roughly in the order of a normal raid.


### 1. Round Start

At the start of a match the deploy / round-start panel appears with white text over a light banner.

The white deploy text triggers the `roundStartWhite` (positive) meter, the light part of the panel is confirmed by `roundStartLight`, and `roundStartNotGrey` (negative) guards the dark part of the UI.

![roundStart](images/roundStart.png)

### 2. In game

Once you are deployed and the battle HUD is visible — the compass across the top, the minimap top-left, the health bar bottom-left — the in-game state activates.

The white compass lettering (top-center) drives `inGameWhite` (positive) and `inGameNotWhite` (negative), while `inGameHealth` reads the far-left of the health bar to confirm the HUD is up. This state gates most other effects, and it is also when the **Health Bar HUD** starts drawing from the `healthLength` reading.

![inGame](images/inGame.png)

### 3. Stamina

Sprint so the stamina bar (lower-center, above the crosshair) appears and drains.

`staminaConfirm` reads the far-left of the stamina bar to confirm it is on screen, and `staminaLength` measures how full the bar is (0–1). The **Stamina** wash reads `staminaLength` live to color green → yellow → red as it drops.

![lowStamina](images/lowStamina.png)


### 4. Low Health

Take damage until your health drops. The health value and its red bar sit at the bottom-left of the screen.

`lowHealthRed` reads the far-left of the health bar for red, and `healthLength` measures the full bar. The **Low Health** heartbeat fires once health falls low and holds — reading `healthLength` live for severity — until you recover. (These same screenshots are also the reference for positioning the **Health Bar HUD**.)

![exposed 2](images/exposed_2.png)


### 5. Action (use / revive / heal)

Perform a timed action — self-heal, revive a teammate, or use a kit — so the action progress bar appears (lower-center, e.g. the "Rescuing…" bar).

`actionConfirm` reads the far-left of the action bar to confirm it is up, `actionNotWhite` (negative) guards just above the bar, and `actionLength` measures how far the action has progressed. The **Action** effect reads `actionLength` live to track the wrap.

![kitUse](images/kitUse.png)

### 6. Kill

Confirm a kill. The orange **Kill** text and the skull / figure icon appear in the center of the screen.

The orange kill text triggers `killOrange` (positive), the skull/figure icon is read by `killIcon` / `killIcon2`, and `killNotOrange` (negative) guards the region directly under the text.

![kill](images/kill.png)


### 7. Low Ammo

Fire until the far-right ammo digit turns red (low / out of ammo).

The red ammo digit triggers `lowAmmoRed` (positive), and `lowAmmoNotRed` (negative) guards directly under it.

![lowAmmo 2](images/lowAmmo_2.png)

### 8. Task / Objective

When a new objective or task banner appears, a wide blue bar is drawn under the task UI.

The wide blue bar triggers `taskBlue` (positive), and `taskNotBlue` (negative) guards the region above the bar.

![task 2](images/task_2.png)

### 9. Downed

Take enough damage to be downed. The red EKG / health icons appear at the bottom-left.

`downedRed1` sits on the red EKG symbol (it never fully fills, so it runs on a lower threshold) and `downedRed2` reads the left side of the downed health bar; `downedNotRed` (negative) guards between the two.

![downed](images/downed.png)

### 10. Dead

Let an enemy finish you. The dead HUD shows the orange plus / health icons. **This state is read while out of game** (the in-game gate has dropped).

`deadOrange` reads the middle of the orange plus, `deadOrange2` reads the left side of the orange health bar, and `deadNotOrange` (negative) guards between them.

![dead 2](images/dead_2.png)

### 11. Extraction (Success / Failed)

Reach the end of a raid. The extraction banner appears full-screen; the grey text under the banner confirms it is up, and the banner color decides the outcome. **Read while out of game.**

* **Success** — the green banner triggers `extractSuccessGreen`, confirmed by `extractGrey` (the grey text under the banner).
* **Failed** — the red **EXTRACTION FAILED** banner triggers `extractFailRed`, confirmed by the same `extractGrey` text meter.

![extract](images/extract.png)

### 12. Inventory

Open the inventory screen (out of game / menu). Both the white and red inventory meters read high together: `inventoryWhite` and `inventoryRed` confirm the inventory UI and trigger the **Inventory** effect.

![inventory](images/inventory.png)

---

## Notes

* **In-game detection gates most effects** — round start, stamina, low health, action, kill, low ammo, task and downed all require the in-game state. **Dead, extraction (success/failed) and inventory are read on their own out-of-game screens**, so they are gated on the in-game state being *off*.
* Some meters are **shared** across states — `extractGrey` confirms both the successful and failed extraction banners, and `killIcon` / `killIcon2` back the same kill icon. Validate them once and they apply everywhere they are reused.
* The **Health Bar HUD** is a persistent overlay whose `healthLength` meter tracks live fill, so its effect reacts to *changes* rather than a fixed level. Use **Bar Effect Adjust** to draw it full-size out of game and position it.
* Detection strategy: every state uses a **positive** meter (the expected color IS present) paired with a **negative "Not"** meter (the region is clear during baseline). A state is active only when positive is high AND negative is low. If an effect never fires, load a target-resolution screenshot in the QA Tool and verify each meter region captures the right pixels at the right color, then adjust the HSL range or coordinates.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

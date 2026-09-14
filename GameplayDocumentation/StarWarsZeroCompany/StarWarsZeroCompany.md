# Star Wars: Zero Company

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Screen resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Testing Guidelines

Most card effects only fire while the integration considers you **in game** (an active tactical HUD is visible) and it is **your turn**. Enter a mission first, then trigger the individual states below.

### 1. In game

Enter a tactical mission. Once the battle HUD is visible — the **ADV** counter and squad portraits at the bottom-left, the **END TURN** button at the bottom-right — the in-game state activates.

This reads the white **ADV** number bottom-left, triggering the `inGameWhite` (positive) and `inGameNotWhite` (negative) meters, together with the `inGameGrey` meter on the surrounding panel.

![ingame](images/ingame.png)

### 2. Shot

Select a unit and choose **Fire Blaster Rifle**. The blue **CONFIRM** card appears at the bottom of the screen.

The blue card triggers the `shotBlue` meter, and the white action label is confirmed by the shared `moveTextWhite` text meter.

![shot](images/shot.png)

### 3. Move

Order a unit to **Move**. The **MOVE** action card appears at the bottom-centre.

The blue dot on the card triggers the `moveBlue` meter, and the white **MOVE** text triggers the `moveTextWhite` meter.

![move](images/move.png)

### 4. Overwatch

Set a unit to **Overwatch**. The **OVERWATCH** action card appears at the bottom.

The blue AP dot on the card triggers the `overwatchBlue` meter, confirmed by the shared `moveTextWhite` text meter.

![overwatch](images/overwatch.png)

### 5. End turn / Begin turn

Press **END TURN**. The end-turn button (bottom-right) is read by four meters that share the same region:

* **End turn** — the button shows its white/lit state: `endTurnWhite` and `endTurnWhite2` go high while `endTurnBlue` drops out.
* **Begin turn** — on the next turn the button returns to its grey resting state: `endTurnGrey` goes high while `endTurnWhite` drops out. Begin turn only fires **after** an end turn has been detected (it is latched), because the same grey/white state also occurs during normal play.

![endturn](images/endturn.png)

### 6. Mission complete

Finish a mission successfully. The **MISSION SUCCESS** banner appears with a blue arrow on each side.

The left and right blue arrows trigger the `missionCompleteBlue1` and `missionCompleteBlue2` meters, and the white banner text triggers the shared `missionTextWhite` meter.

![missioncomplete](images/missioncomplete.png)

### 7. Mission failed

Fail a mission. The **MISSION FAILED** banner appears with a red arrow on each side.

The left and right red arrows trigger the `missionCompleteRed1` and `missionCompleteRed2` meters (same regions as mission complete, tuned for red), confirmed by the shared `missionTextWhite` banner-text meter.

![missionfailed](images/missionfailed.png)

### 8. Reinforcements

Play until the **REINFORCEMENTS INCOMING** banner appears at the top-centre of the screen.

The white banner text triggers the `reinforcementsTextWhite` (positive) meter, guarded by the `reinforcementsNotWhite` (negative) meter next to it.

![reinforcements](images/reinforcements.png)

### 9. Death

Let one of your operatives be killed. The black **… HAS DIED** card appears with the name in red.

The red death text triggers the `deathTextRed` (positive) meter, the `deathNotRed` (negative) meter guards the region beside it, and `deathBlack` confirms the dark card background.

![death](images/death.png)

### 10. Downed / Recovery

*(No dedicated card — read from the in-game HUD, so use the **In game** screen above.)*

While in game and on **your turn**, a downed teammate raises the red health bar under the squad portraits (bottom-left). This is read as a **bar**: roughly every `0.2` of fill is one downed operative (cap 3 — a 4th is a death).

* A step **up** in the bar triggers the `downedRed` (positive) meter → **Downed**.
* A step **down** (the operative is healed) → **Recovery**.
* `downedNotRed` (negative) guards the reading. A confirmed **Death** is not counted as a down.

![ingame](images/downed.png)

---

## Notes

* **Card effects require both "in game" and "your turn".** If an effect never fires, first confirm the integration is registering the in-game / turn state (step 1 and step 5).
* Some meters are **shared** across states — `moveTextWhite` backs Shot, Move and Overwatch, and `missionTextWhite` backs both Mission Complete and Failed. Validate them once and they apply everywhere they are reused.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and colour detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

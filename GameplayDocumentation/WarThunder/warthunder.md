# War Thunder

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

Verify that the selected resolution appears in the Screen resolution option, then set the game to the resolution being tested. If it is not available, adjust the Aspect Ratio setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Vehicle Selection

War Thunder reads a different HUD depending on the vehicle you are driving, so the integration exposes a **Vehicle selection** control (`Tank`, `Plane`, `Ship`).

**Before each test, set this control to match the vehicle you are actually using.** Several meters (in-game detection, repair, team advance, low tide, plane overload) only fire for the matching vehicle type, so a wrong selection will make effects appear to never trigger.

---

## Testing Guidelines

Observation: Most effects only fire while the integration considers you "in game" (an active battle HUD is visible). Enter a battle first, then trigger the individual states below.

### Tank

1. Set **Vehicle selection** to **Tank** and enter a Ground battle (a training area or Test Drive also works).

2. Once the battle HUD is visible — top team bars (blue/red) and the tank UI on screen — the in-game state activates, triggering the `inGameWhite` and `inGameNotWhite` meters.

3. The top team bars drive team detection. When they are solid they feed the `teamBlue` / `teamRed` meters (used for team advance, zoom and win/loss palette).

![team](images/team.png)

5. Zoom your gun sight (the top team bars drop out of the HUD) .the second team-bar set meters will appear (`teamBlue2` / `teamRed2`)

![team2](images/team2.png)

6. When a "capturing zone" / team-action banner appears at the bottom of the screen, the `teamTextBlue` / `teamTextRed` meters trigger(color matches the capturing team).

![textteam](images/textteam.png)
![textteam2](images/textteam2.png)

7. Take some hits so that will start repair,triggers the `tankRepairWhite` and `tankRepairNotWhite` meters.

![repair](images/repair.png)

8. Capture a point. The blue capture indicator triggers the `captureBlue` and `captureNotBlue` meters.

![capture](images/capture.png)

9. Let an enemy destroy you.  triggers the `deathRed` and `deathNotRed` meters.

![death](images/death.png)

10. Complete the match. The **Mission Accomplished** banner triggers the `missonCompleteRed` and `missionCompleteNotRed` meters. 

![mission](images/mission.png)

### Ship

11. Set **Vehicle selection** to **Ship** and enter a Naval battle.

14. Get some hits to be able to repair your ship to trigger the `boatRepairWhite` and `boatRepairNotWhite` meters.

![boatrepair](images/boatrepair.png)

15. Sail into shallow water until the low-depth warning appears. triggers the `boatLowYellow` / `boatLowNotYeloow` meters. When the warning escalates to its red/critical variant, the `boatLowRed` / `boatLowNotRed` meters.

![boatlow](images/boatlow.png)
![boatlow2](images/boatlow2.png)

### Plane

16. Set **Vehicle selection** to **Plane** and enter an Air battle.

17. Pull a hard maneuver until the red **OVERLOAD** warning appears. This triggers the `planeOverloadRed` meter.

![planeoverload](images/planeoverload.png)

---

## Notes

* **Vehicle selection must match the vehicle in use** — repair, in-game detection, low tide and plane overload are gated by vehicle type and will not fire otherwise.
* Meter validation alone does not guarantee the integration is working. A complete test should verify both:
  * Meter validation (correct region and color detection)
  * Effect validation (the lighting effect actually triggers and looks correct)

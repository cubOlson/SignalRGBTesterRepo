# Palworld

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

Observation: If you don't have a save yet, start a new game and play through character creation. Most meters only fire while the integration considers you "in game" (the compass at the top of the screen is visible), so load into the world first, then trigger the individual states below.

1. Load into the world so the compass is visible at the top center of the screen. This activates the in-game detection, triggering the `inGameCompassWhite`, `inGameCompassLong` and `inGameCompassNotWhite` meters.
![inGameCompass](images/inGameCompass.png)

2. With the HUD visible in the bottom-left corner, the three status bars are read. Take some damage so the bars drop and refill to trigger the health, shield and hunger meters:
   - Health bar → `health`, `healthRed1`, `healthRed2`, `healthWhite`
   - Shield bar → `shield`, `shieldRed1`, `shieldRed2`
   - Hunger bar → `hunger`, `hungerWhite`
![statusBars](images/statusBars.png)


3. Let an enemy hit you (or your shield) so the bar drains sharply. The drop triggers the damage streak effect, driven by the `healthRed1` / `healthRed2` and `shieldRed1` / `shieldRed2` meters.
![damage](images/damage.png)
![damage2](images/damage2.png)

4. Summon your partner Pal so its health bar appears next to the HUD. This triggers the `palBlue`, `palRed1` and `palRed2` meters. Letting the Pal take damage / faint drives the same meters.
![pal](images/pal.png)

5. Throw a Pal Sphere at a wild Pal. The throw/confirm animation triggers the `palSphereColor`, `palSphereConfirm` and `palSphereNotColor` meters.
![palSphere](images/palSphere.png)

6. Successfully capture a wild Pal. The capture banner triggers the `captureBlue`, `captureWhite0`, `captureWhite1`, `captureWhite2` and `captureNotWhite` meters.
![capture](images/capture.png)

7. Gain enough experience to level up. The level-up popup triggers the `levelUp` and `levelUpNotBlue` meters.
![levelUp](images/levelUp.png)

8. Wait for the in-game clock to advance so the day/night indicator changes. Daytime triggers the `dayTimeWhite` meter; the transition/night state triggers the `timeYellow` meter.
![time](images/time.png)

9. Enter one of your bases (an area flagged as your base). This triggers the `baseGrey` and `baseWhite` meters.
![base](images/base.png)

10. Engage a Boss / Alpha Pal so its boss health bar appears at the top of the screen. This triggers the `bossRed` and `bossNotRed` meters.
![boss](images/boss.png)

11. Let an enemy kill you. The death screen triggers the `deathBlack`, `deathRed1` and `deathRed2` meters.
![death](images/death.png)

---

## Notes

* Most meters are gated by the in-game / HUD detection (compass + bottom-left HUD visible). If the compass or HUD is hidden, health, shield, hunger, pal, sphere and damage effects will not fire.
* The Health, Shield and Hunger HUD readers each have their own toggle and X/Y/width/height controls, plus a "HUD Black Background" option — if a bar is misread, check that the HUD overlay is aligned to the on-screen bar.
* Effect groups can be toggled independently in the integration (Pal, Day/Night, Level Up, Death, Hunger, Base, Boss). Make sure the relevant effect is enabled before testing its meter.
* Meter validation alone does not guarantee the integration is working. A complete test should verify both:
  * Meter validation (correct region and color detection)
  * Effect validation (the lighting effect actually triggers and looks correct)

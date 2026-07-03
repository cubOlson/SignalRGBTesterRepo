# Marvel Rivals

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

Open the video settings.

Verify that the selected resolution is correct.

The game should be configured to use the desired test resolution before creating a Custom Game.

---

## Testing Guidelines

1. Launch the game.

2. Select:

   * **Play**
   * **Change Mode**
   * **Custom Game**
   * **Create**

3. Configure the teams as follows:

   * Fill **your team** with **Easy AI**.
   * Fill the **enemy team** with **Hard AI**.

   This setup allows the match to end quickly so the defeat meters can be validated.

4. Select any hero.

5. At the start of the match, the following meters will trigger automatically:

   * **InGameWhite**
   * **InGameNotWhite**
   * **HealthWhite**
   * **HealthConfirm**
   * **HealthWhite**
   * **UltBlue**
   * **UltNotBlue**
   

![In Game HUD](images/in-game-hud.png)

6. Gain a temporary shield.

   This triggers:

   * **HealthBlue**

![Shield Meter](images/shield-meter.png)

7. Take damage.

   This triggers:

   * **HealthRed**

![Health Damage](images/health-damage.png)

8. Eliminate an enemy player.

   This triggers:

   * **KillNotRedLeft**
   * **KillNotRedRight**
   * **KillRedBottom**
   * **KillRedTop**

![Kill Meter](images/kill-meter.png)

9. Allow the enemy team to win the match.

    This triggers:

* **DefeatDarkGray**
* **DefeatLightGray**

![Defeat Screen](images/defeat-screen.png)

10. Create another Custom Game.

11. Reverse the AI difficulty:

* Fill **your team** with **Hard AI**.
* Fill the **enemy team** with **Easy AI**.

12. Win the match.

    This triggers:

* **VictoryOrange**
* **VictoryYellow**

![Victory Screen](images/victory-screen.png)

---

## Game Mode Testing

Create a separate Custom Game for each of the following game modes.

### Doom Match

13. Select **Arcade → Doom Match**.

    This triggers:

* **DoomMatchBlue**
* **DoomMatchOrange**

![Doom Match](images/doom-match.png)

### Conquest

14. Create a new Custom Game using **Conquest**.

    This triggers:

* **ConquestBlue**
* **ConquestOrange**

![Conquest](images/conquest.png)

### Convoy (Push)

15. Create a new Custom Game using **Convoy**.

    This triggers:

* **PushGrayDiamond**
* **PushNotGrayDiamond**

![Convoy](images/convoy.png)

### Domination

16. Create a new Custom Game using **Domination**.

    This triggers:

* **ZoneCheckBlue**
* **ZoneCheckNotBlue**

![Domination](images/domination.png)

---

## Overtime Testing

17. Create another Custom Game.

18. Configure **both teams** with **Easy AI**.

This creates a more balanced match, increasing the chance of reaching Overtime.

19. Allow the objective to remain contested until Overtime begins.

    This triggers:

* **ZoneOvertime**
* **ZoneNotOvertime**

![Overtime](images/overtime.png)

---

## Notes

* Each game mode must be tested separately, as several meters are mode-specific.
* The recommended AI difficulty is intended to speed up testing by making it easier to intentionally lose or win matches.
* Meter validation alone does not guarantee that the integration is functioning correctly.
* In addition to verifying meter placement and activation, testers should also verify that the corresponding lighting effects trigger correctly.
* A complete integration test should include both:

  * Meter validation
  * Effect validation
* If a meter triggers correctly but the expected lighting effect does not appear, the integration should still be investigated further.

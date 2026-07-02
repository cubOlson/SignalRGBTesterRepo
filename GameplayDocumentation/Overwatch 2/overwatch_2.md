# Overwatch 2

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

The game should be configured to use the desired test resolution before entering the Practice Range.

---

## Testing Guidelines

1. Launch the game.

2. Select:

   * **Play**
   * **More**
   * **Practice Range**

3. Select any hero.

4. After entering the Practice Range, the following meters will trigger automatically:

   * **HealthBarLength**
   * **HealthBarValue**
   * **InGameBlueBottom**
   * **InGameBlueLeft**
   * **InGameBlueRight**

![Practice Range HUD](images/practice-range-hud.png)

5. Eliminate one training bot.

   This triggers:

   * **KillMiddleRed**
   * **KillWhite**

![Kill Meter](images/kill-meter.png)

6. Continue eliminating bots until your Ultimate reaches 100%.

   This triggers:

   * **UltBlue**
   * **UltWhite**

![Ultimate Ready](images/ultimate-ready.png)

7. As you gain enough progression during the match, the following meter may also trigger:

   * **PerkBarOrange**

![Perk Meter](images/perk-meter.png)

8. Allow the training bots to damage you.

   This triggers:

   * **AntiHeal**

![Anti Heal](images/anti-heal.png)

9. Move away from combat and allow your health to regenerate.

   This triggers:

   * **GettingHealedTR**

![Healing](images/healing.png)

10. Verify the hero ability meters.

    The following meters are hero-dependent:

* **SmallBlockOrange**
* **SmallGroupWhite**

These meters use image recognition and may appear in different locations depending on the selected hero.

---

## Hero-Specific Testing

### Sigma

11. Select **Sigma**.

12. Use Sigma's Barrier ability (Right Mouse Button).

    This triggers:

* **SigmaBlockOrange**

![Sigma Ability2](images/sigma-ability2.png)

13. Recall or redeploy the barrier.

    This triggers:

* **SigmaWhite**

![Sigma Ability](images/sigma-ability.png)

---

### Zenyatta

14. Select **Zenyatta**.

15. Approach an enemy bot and use **E** (Orb of Discord).

    This triggers:

* **ZenPurple**

![Zenyatta Discord](images/zen-discord.png)

16. Target a friendly bot and use **Left Shift** (Orb of Harmony).

    This triggers:

* **ZenYellow**

![Zenyatta Harmony](images/zen-harmony.png)

---

### Freja / Wrecking Ball

17. Select either:

* **Freja**
* **Wrecking Ball**

18. Trigger the corresponding hero-specific ability.

    This triggers:

* **WreckingFrejaWhite**

![Freja Meter](images/freja-meter.png)

---

## Match Testing

19. Return to the main menu.

20. Select:

* **Play**
* **More**
* **Custom Games**

21. Create a new Custom Game.

22. Configure the teams as follows:

* Fill **your team** with **Beginner AI**.
* Fill the **enemy team** with **Hard AI**.

This configuration makes it easier to lose the match and validate the defeat meters.

23. Select any hero and start the match.

24. Allow the enemy team to win.

    This triggers:

* **DefeatRed**
* **DefeatBlack**

![Defeat Screen](images/defeat-screen.png)

25. Create another Custom Game.

26. Reverse the team difficulty:

* Fill **your team** with **Hard AI**.
* Fill the **enemy team** with **Beginner AI**.

27. Start the match and allow your team to win.

    This triggers:

* **VictoryCyan**
* **VictoryWhite**

![Victory Screen](images/victory-screen.png)

---

## Overtime Testing

28. Create another Custom Game.

29. Configure both teams with **Beginner AI**.

This creates a more balanced match, increasing the chance of reaching **Overtime**.

30. Start the match and allow the objective to be contested until Overtime begins.

    This triggers:

* **OvertimeBar**
* **OvertimeWhite**

![Overtime](images/overtime.png)


---

## Notes

* The Practice Range is sufficient to validate the general integration meters and several hero-specific meters.
* The **SmallBlockOrange** and **SmallGroupWhite** meters are based on image recognition and therefore appear in different positions depending on the selected hero.
* These hero-dependent meters are calibrated using reference images generated with image editing tools such as Photoshop or GIMP.
* Additional hero-specific meters may require testing in standard game modes and are documented separately.
* Meter validation alone does not guarantee that the integration is functioning correctly.
* In addition to verifying meter placement and activation, testers should also verify that the corresponding lighting effects trigger correctly.
* A complete integration test should include both:

  * Meter validation
  * Effect validation

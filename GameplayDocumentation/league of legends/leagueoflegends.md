# League of Legends

## Supported Resolutions

- 1920x1080
- 1920x1200
- 2560x1080
- 2560x1440
- 2560x1600
- 3440x1440
- 3840x1080
- 3840x2160
- 5120x1440
- 3840x2160 (HDR)

---

## Resolution Setup

Open the video settings.

Verify that the selected resolution is correct.

The game should be configured to use the desired test resolution before entering a match.

---

## Testing Guidelines

1. Launch the game and enter:
   - **Training**
   - **Practice Tool**

2. Create a match with:
   - Several bots on your team
   - One Intro-level bot on the enemy team

3. Start the game and select any champion.

   Lucian is commonly used for testing, but any champion can be selected.

4. After loading into the match, the following meters will trigger automatically:
   - **allyDeath1**
   - **allyDeath2**
   - **allyDeath3**
   - **allyDeath4**
   - **InGame1**
   - **InGame2**
   - **Recall**
   - **Low Health**

![In Game HUD](images/in-game-hud.png)

4. After loading into the match, the following meters will trigger automatically:
   - **allyDeath1**
   - **allyDeath2**
   - **allyDeath3**
   - **allyDeath4**
   - **InGame1**
   - **InGame2**
   - **Recall**
   - **LowHealth**

![In Game HUD](images/in-game-hud.png)

5. Press:

   **Shift + T**

   This grants gold and triggers:
   - **Gold**
   - **GoldHundred**
   - **GoldThousand**

![Gold Meter](images/gold-meter.png)

6. Buy some items and hover your mouse over an item in your inventory.

   This will trigger:
   - **itemHover**

![Item Hover Meter](images/itemhover.png)

7. Press:

   **Shift + Y**

   This levels up your champion to the maximum level and triggers:
   - **ExtendedQButton**
   - **ExtendedWButton**
   - **ExtendedEButton**
   - **ExtendedRButton**

![Extended Abilities Meter](images/level-up-meter.png)

8. After leveling up all abilities, trigger:
   - **LevelUp**
   - **qButton**
   - **wButton**
   - **eButton**
   - **rButton**
   - **dButton**
   - **fButton**

![Level Up Meter](images/level-up2-meter.png)

9. Press:

   **Shift + X**

   This enables unlimited resources. Use your abilities repeatedly until your mana bars are consumed and restored to trigger:

   - **ManaQ**
   - **ManaW**
   - **ManaE**
   - **ManaR**

![Mana Meter](images/mana-meter.png)

10. While using abilities, some champions will display a charge or cast bar.

    This triggers:
   - **LoadBar**

![LoadBar Meter](images/loadbar.png)

11. Press **B** to Recall.

    This triggers:
   - **BlueBar**

![BlueBar Meter](images/bluebar.png)

12. Allow an enemy turret to damage you.

    This triggers:
   - **tookDamage**

![Took Damage Meter](images/took-damage-meter.png)

13. Destroy the first enemy turret.

    This triggers:
   - **firstEnemyTurret**
   - **blueConformation**

![First Turret Destroyed](images/first-turret.png)

14. Destroy another enemy turret.

    This triggers:
   - **enemyTurret**

![Enemy Turret Destroyed](images/second-turret.png)

15. Wait for Dragon to spawn.

    This triggers:
   - **GrayDragon**

![Gray Dragon](images/gray-dragon.png)

16. Wait for Dragon progress to activate:

   - **YellowDragon**

![Yellow Dragon](images/yellow-dragon.png)

17. Wait for Baron Nashor to spawn.

    This triggers:
   - **GrayBaron**

![Gray Baron](images/gray-baron.png)

18. Wait for Baron progress to activate:

   - **YellowBaron**

![Yellow Baron](images/yellow-baron.png)

19. End the match after all objectives have been validated.

---

## Enemy Push Testing

20. Start a new Practice Tool match.

21. Do not add bots to your team.

22. Fill the enemy team with bots.

23. Start the game and allow the enemy team to push naturally.

24. Wait for the enemy team to destroy:

   - **firstAllyTurret**
   - **allyTurret**

![Enemy Turret 1 Push](images/enemy1-push.png)
![Enemy Turret 2 Push](images/enemy2-push.png)
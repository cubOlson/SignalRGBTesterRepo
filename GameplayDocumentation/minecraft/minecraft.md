# MInecraft

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

1. Launch the game and load the Minecraft test world.

2. After loading into the world, the following meters will trigger automatically:

   - **InGame**
   - **InGameNot**
   - **Health**
   - **FoodBar**
   - **LowHP**

![In Game HUD](images/in-game-hud.png)

3. Go to the Enchanting Table and open it.

   This triggers:
   - **EnchantmentTableBrown**
   - **InMenuBlack**
   - **InMenuWhite**

![Enchanting Table](images/enchanting-table.png)

4. Go to the Anvil (Repair Table) and open it.

   This triggers:
   - **AnvilWhiteHammer**

![Anvil](images/anvil.png)

5. Go to the Furnace and open it.

   This triggers:
   - **InFurnaceGreen**

![Furnace](images/furnace.png)

6. Open the recipe book while inside the Furnace UI.

   This triggers:
   - **InFurnaceGreenOpenBook**

![Furnace Open Book](images/furnace-open-book.png)

7. Open the Upgrade Gear (Smithing Table) station.

   This triggers:
   - **SmithingTable**

![Smithing Table](images/smithing-table.png)

8. Open the Crafting Table.

   This triggers:
   - **InCraftingGreen**

![Crafting Table](images/crafting-table.png)

9. Open the recipe book while inside the Crafting Table UI.

   This triggers:
   - **InCraftingGreenOpenBook**

![Crafting Open Book](images/crafting-open-book.png)

10. Open the Large Chest.

   This triggers:
   - **LChestBlack**
   - **LChestWhite**

![Large Chest](images/large-chest.png)

11. Open the Small Chest.

   This triggers:
   - **SChestBlack**
   - **SChestWhite**

![Small Chest](images/small-chest.png)

12. Mount the horse.

   This triggers:
   - **MountOrange**
   - **MountBlack**

![Horse Mount](images/horse-mount.png)

13. Unlock the available recipes.

   This triggers:
   - **NewRecipePurple**
   - **NewRecipeWhite**

![Recipe Unlock](images/recipe-unlock.png)

14. Enter the water and remain submerged long enough to trigger:

   - **Bubble1**
   - **Bubble2**
   - **Bubble3**

![Bubble Meter](images/bubble-meter.png)

15. Use the Ice Block section.

   This triggers:
   - **HealthB**

![Health B Meter](images/health-b.png)

16. Enter the Bad Effects area.

   This triggers:
   - **BadAccent**
   - **BadMain**
   - **BadStatusBlack**
   - **BadStatusGray**

![Bad Effects](images/bad-effects.png)

17. While inside the poison effect area, trigger:

   - **HealthG**

![Health G Meter](images/health-g.png)

18. Use the XP test area.

   This triggers:
   - **XP**

![XP Meter](images/xp-meter.png)

19. Enter the Good Effects area.

   This triggers:
   - **GoodAccent**
   - **GoodMain**
   - **GoodStatusBlack**
   - **GoodStatusGray**

![Good Effects](images/good-effects.png)

20. Change the world time to night.

21. Use the bed.

   This triggers:
   - **SleepBlack**
   - **SleepWhite**

![Sleep Meter](images/sleep-meter.png)

22. Enter the next room and jump into the pool.

   This triggers:
   - **Boss**
   - **BossWhite**

![Boss Meter](images/boss-meter.png)

## Notes
- This test world is designed to expose the locations and trigger conditions for the integration meters. 
- The purpose of this documentation is to help validate that the correct meters appear and trigger at the expected locations. 
- Meter validation alone does not guarantee that the integration is functioning correctly. 
- In addition to verifying the meters, testers should also verify that the corresponding lighting effects are triggering correctly and are visually behaving as expected. 
- A complete integration test should include both: 
    - Meter validation 
    - Effect validation 
- If a meter triggers correctly but the expected lighting effect does not appear, the integration should still be considered for further investigation.
# Slay the Spire 2

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

Open the graphics settings.

Verify the following settings:

* **Fullscreen:** Disabled
* **Aspect Ratio:** Auto

Select the resolution currently being tested.

This allows the integration to properly adapt to all supported resolutions.

---

## Testing Guidelines

1. Launch the game.

2. If the **Abandon Run** option is available, select it.

   This will reset the current run and allow testing to begin from the start.

3. Select:

   * **SinglePlayer**
   * **Standard**

   This triggers:

   * **ActionBlue**
   * **ActionRed**
   * **ActionWhite**

![Main Menu](images/main-menu.png)

4. Select any character.

5. When Neow appears, the following meters will trigger:

   * **NPCBlue**
   * **NPCYellow**
   * **NPCDark**
   * **CoinYellow**
   * **GearGray**

![NPC Event](images/npc-ingame.png)

6. Select any starting option and click **Proceed**.

7. Open the map.

   This triggers:

   * **ElitePurple**
   * **UnknownYellow**

![Map Overview](images/map-overview.png)

8. Plan a route that reaches the following locations as quickly as possible:

   * **Merchant**
   * **Unknown**
   * **Rest**

   These locations are required to validate their respective meters later in the run.

9. Enter the first combat encounter.

   This triggers:

   * **BattleYellow**
   * **TurnBlue**

![Combat Start](images/combat-start.png)

10. During combat, after using all available cards, end your turn.

    This triggers:

   * **ManaRed**

![Mana Meter](images/mana-meter.png)

11. Win a combat encounter.

    This triggers:

   * **BannerBrown**
   * **BannerNotBrown**
   * **EntryRed**
   * **LootBlue**
   * **LootNotBlue**

![Combat Rewards](images/combat-rewards.png)

12. Add a card to your deck or skip the reward.

    This triggers:

   * **SkipBlue**
   * **SkipNotBlue**

![Card Reward](images/card-reward.png)

13. Return to the map and continue progressing toward the required nodes:

   * **Unknown**
   * **Merchant**
   * **Rest Site**

---

## Unknown Event Testing

14. Enter an Unknown event.

    This triggers:

   * **RandomYellow**
   * **NotYellow**

![Unknown Event](images/unknown-event.png)

---

## Merchant Testing

15. Enter a Merchant node.

    This triggers:

   * **EntryGreen**

![Merchant Entry](images/merchant-entry.png)

16. Select the Merchant.

    This triggers:

   * **ShopGreen**
   * **ShopNotGreen**
   * **ShopYellow**

![Merchant Shop](images/merchant-shop.png)

17. Purchase the yellow coin service (Card Removal).

18. Select any card from your deck and remove it.

    This triggers:

   * **CutRed**

![Card Removal](images/card-removal.png)

---

## Rest Site Testing

19. Enter a Rest Site.

    This triggers:

   * **RestGreen**
   * **RestRed**

![Rest Site](images/rest-site.png)

---

## Death Testing

20. After obtaining the Rest, Unknown, and Merchant meters, continue into another combat encounter.

21. Allow your character to die.

    The easiest method is to continue ending turns without defending or healing until defeated.

22. Upon death, the following meters will trigger:

   * **DeadRed1**
   * **DeadRed2**

![Death Screen](images/death-screen.png)

---

## Notes

* This documentation is intended to identify the locations and trigger conditions for the integration meters.
* Meter validation alone does not guarantee that the integration is functioning correctly.
* In addition to verifying meter placement and activation, testers should verify that the corresponding lighting effects trigger correctly.
* A complete integration test should include both:
  * Meter validation
  * Effect validation
* If a meter triggers correctly but the expected lighting effect does not appear, the integration should still be investigated further.
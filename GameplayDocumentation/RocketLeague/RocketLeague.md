# Rocket League

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

Observation: Most in-match effects only fire while the integration considers you "in game" (the match HUD with the boost gauge is visible). The Search and End Game effects fire outside of a match (menu / result screen), so test those at the start and end of a game.

1. From the main menu, start looking for a match. The searching screen triggers the `searchingBot` and `searchingMid` meters (Search effect).
![search](images/search.png)

2. Load into a match so the HUD and boost gauge are visible. This activates the in-game detection, triggering the `inGameBlack` and `inGameWhite` meters.

3. Drive over boost pads to fill your boost gauge. As the gauge fills it steps through the `boost1`, `boost2`, `boost3`, `boost4`, `boost5`, `boost6` and `boost7` meters (Boost effect).
![boost](images/boost.png)

4. Score a goal. The goal explosion triggers the `scoreBall`, `scoreLeft` and `scoreRight` meters (Goal effect — the color follows the scoring team). `scoreLeft` / `scoreRight` read which side scored.
![goal](images/goal.png)

   After the goal, the replay plays with letterbox bars — this is read by the `replayBlock` meter (helper meter used to detect the replay and suppress duplicate goal effects; it does not drive a lighting effect on its own).
![replayBlock](images/replayBlock.png)

5. Block a shot on your net to get a **Save!** popup, triggering the `saveLeft`, `saveRight` and `saveNotOrange` meters (Save effect).
![save](images/save.png)

6. Make an **Epic Save!** (a save close to the goal line) to trigger the `epicSaveLeft`, `epicSaveRight` and `epicSaveNotOrange` meters.
![epicSave](images/epicSave.png)

7. Get demolished (or demolish an opponent) so the **Demolished** popup appears, triggering the `demo`, `demo2` and `demoNotOrange` meters.
![demo](images/demo.png)

8. Spam a quick chat (e.g. "What a save!") so the chat feed fills. This triggers the `QuickChatWhite` and `QuickChatBlue` meters (Chat Spam effect).
![quickChat](images/QuickChat.png)

9. Take the match to **Overtime**. The overtime banner triggers the `overtime` meter (Overtime effect).
![overtime](images/overtime.png)

10. Finish the match and reach the result screen with a **win**. This triggers the `win` and `winNotWhite` meters (End Game effect — fires on the post-match screen, not during play).
![win](images/win.png)

---

## Notes

* The Search and End Game effects fire while you are **not** in a match (menu / result screen). Every other effect requires the in-game detection (match HUD visible) to be active.
* The Goal effect has a **Goal Effect Choice** (Ball / Spots / Swirl) and the boost HUD has its own color / position / style controls — set these before testing so the effect you expect is the one that plays.
* Effect groups can be toggled independently in the integration (Boost, Goal, Save / Demo, Chat Spam, Overtime, End Game, Search). Make sure the relevant effect is enabled before testing its meter.
* Meter validation alone does not guarantee the integration is working. A complete test should verify both:
  * Meter validation (correct region and color detection)
  * Effect validation (the lighting effect actually triggers and looks correct)

# Rocket League

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160

The integration also carries aspect-ratio fallbacks (**3.55:1**, **24:10** and **16:11**), so other resolutions sharing those aspect ratios are read from the nearest matching profile. **2560x1080** and **3840x2160** also have their own dedicated profiles. HDR color ranges are defined for every meter.

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Display Mode** / aspect ratio so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Boost Effects**, **Goal Effects**, **Save / Demo Effects**, **Chat Spam Effect**, **Overtime Effect**, **End Game Effects** and **Search Effects**. **Goal Effect Choice** selects the goal animation (**Ball**, **Spots** or **Swirl**), and **Ambience brightness** sets how bright the screen-ambience background is.

The **Boost HUD** has color, X/Y position and size controls, and a **HUD style** switch between a circle (radius) and a rectangle (width/height). Turn on **Adjust Boost HUD** to show the HUD shape at full size so it can be placed on the keyboard, and **turn it off in game**.

Most effects only fire while the integration considers you **in game** (see step 1). The exceptions are **Search**, **Chat Spam** and **Replay** detection, which read outside it, and **End Game**, which fires while the in-game state is *off*.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

Test images live in `SDR/<meter>/<meter>_<op><value>.png`. The screenshots were captured with the game in Portuguese; detection is by color and position, so the language does not matter.

### 1. In game (gate)  ·  `inGameBlack`, `inGameWhite`

The in-game state turns on when the match clock at the top-center reads: the dark background (`inGameBlack` >= .95) **and** the white clock digits (`inGameWhite` >= .95). It turns off as soon as either one is lost. **Most effects require this state.** Both meters use the same screenshot.

![in game](images/in_game.png)

### 2. Boost  ·  `boost1` … `boost7`

The circular boost gauge at the bottom-right. Seven sample points along the ring (`boost1` at the bottom up to `boost7` at the top) each read lit when the boost has reached that level, giving 8 states from **0** (empty) to **7** (full):

* A lit point counts when it and the one below it read >= .95, and the next one up reads < .1 (the `boost7` / `boost6` pair alone means full).
* The **Boost HUD** shows the level on the keyboard (a bar or a pie, depending on **HUD style**). At **0** it pulses red, and at **7** it pulses in the chosen **Boost Color**.
* Every time the level goes **up**, the **Boost Up** flash plays (a white expanding square or ring).
* The HUD is hidden for a few seconds after a goal (see step 5).

All seven meters use the same screenshot (full boost shown). Test partial levels live by spending boost and picking pads up.

![boost](images/in_game.png)

### 3. Save  ·  `saveLeft`, `saveRight`, `saveNotOrange`

The **SAVE** callout at the top-center: the left and right orange markers (`saveLeft` >= .95 **and** `saveRight` >= .95) with `saveNotOrange` < .1, while the ball icon is in the event feed (`scoreBall` >= .2). Plays the **Save** effect (yellow rings expand from the center). It has a 2 second cooldown.

![save](images/save.png)

### 4. Epic Save  ·  `epicSaveLeft`, `epicSaveRight`, `epicSaveNotOrange`

The **EPIC SAVE** callout, one line lower than a regular save: `epicSaveLeft` >= .95 **and** `epicSaveRight` >= .95 with `epicSaveNotOrange` < .1 and `scoreBall` >= .2. Plays the **Epic Save** effect (rainbow rings expand from the center). It has a 2 second cooldown.

![epic save](images/epic_save.png)

### 5. Goal  ·  `scoreLeft`, `scoreRight`, `scoreBall`

The score boxes at the top-center flash when a goal is scored. Each box is read as a color (hue, saturation and lightness). A goal is detected when, with the ball icon in the event feed (`scoreBall` > .1), a box's **lightness rises** while its **saturation drops by more than 5** (the white flash). Plays the **Goal** effect for the chosen style, in the color of the team that scored:

* **Ball** — a ball rolls across the keyboard toward the scoring side and bursts at the center
* **Spots** — expanding rings pop at random positions
* **Swirl** — three arms spiral outward

After a goal the Boost HUD is hidden until the replay ends (see step 9) or 15 seconds pass.

Goal flash is change-based, so validate it **live** rather than from a static frame.

Score boxes:

![goal](images/goal.png)

Ball icon in the event feed (`scoreBall`):

![goal ball](images/goal_ball.png)

### 6. Demolition  ·  `demo`, `demo2`, `demoNotOrange`

The **DEMOLITION** callout at the top-center: the two orange bars (`demo` >= .95 **and** `demo2` >= .95) with `demoNotOrange` < .1. Plays the **Demo** effect (an orange explosion bursts from the center three times). It has a 2 second cooldown.

![demo](images/demo.png)

### 7. Overtime  ·  `overtime`

The **+** sign in front of the match clock when the game goes to overtime: `overtime` >= .95. Plays the **Overtime** effect (a red pulse over the keyboard) for as long as the sign is visible (<= .1 ends it).

![overtime](images/overtime.png)

### 8. Chat spam  ·  `QuickChatBlue`, `QuickChatWhite`

Quick-chat spam: the chat box is open (`QuickChatBlue` >= .95) **and** the quick-chat list is showing (`QuickChatWhite` >= .75). Plays the **Chat Spam** effect (a green wash with ripples, the "What a save!" look). It re-arms once both are gone. This does not require the in-game state.

Chat box:

![quick chat input](images/quick_chat_input.png)

Quick-chat list:

![quick chat menu](images/quick_chat_menu.png)

### 9. Replay  ·  `replayBlock`

The red **REPLAY** label at the bottom-left after a goal: `replayBlock` >= .95. It has no effect of its own; when it disappears, the goal's Boost HUD block ends and the HUD returns.

![replay](images/replay.png)

### 10. Match found / searching  ·  `searchingMid`, `searchingBot`

The **searching for a match** screen: the animated bar in the middle (`searchingMid` >= .95) **and** the blue bar at the bottom (`searchingBot` >= .95). Plays the **Search** effect (a cyan hexagon with arrows sweeping out), and stops when either drops below .75. This does not require the in-game state.

![searching](images/searching.png)

### 11. Win  ·  `win`, `winNotWhite`

The end-of-match **VICTORY** screen: the trophy icon at the top-right (`win` >= .95) with `winNotWhite` < .1 and the ball icon visible (`scoreBall` >= .3), **while the in-game state is OFF**. Plays the **End Game** effect (rainbow rings rise from the bottom for 15 seconds).

![win](images/win.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (match clock) | `inGameBlack`, `inGameWhite` |
| Boost gauge | `boost1`, `boost2`, `boost3`, `boost4`, `boost5`, `boost6`, `boost7` |
| Save | `saveLeft`, `saveRight`, `saveNotOrange` |
| Epic Save | `epicSaveLeft`, `epicSaveRight`, `epicSaveNotOrange` |
| Goal (score boxes + event feed) | `scoreLeft`, `scoreRight`, `scoreBall` |
| Demolition | `demo`, `demo2`, `demoNotOrange` |
| Overtime clock | `overtime` |
| Chat spam | `QuickChatBlue`, `QuickChatWhite` |
| Replay label | `replayBlock` |
| Searching | `searchingMid`, `searchingBot` |
| Win screen | `win`, `winNotWhite` |

**29 meters across 11 screens** driving 9 effects (Boost HUD, Boost Up, Goal, Save, Epic Save, Demo, Chat Spam, Overtime, Search, End Game).

---

## Notes

* **In-game detection gates most effects.** Search and Chat Spam read outside it, and the Win screen only fires while the gate is OFF.
* **Positive / negative pairs** — most states use a positive meter plus a `…NotOrange` / `…NotWhite` negative guard (e.g. `saveLeft` + `saveNotOrange`). Both must read correctly for the effect to fire cleanly.
* **Save, Epic Save and Win also need the ball icon** (`scoreBall`) in the event feed, so a callout without its ball icon will not trigger.
* **Cooldowns** — Save, Epic Save and Demo ignore repeats for 2 seconds.
* **Goal and Boost are change-based** — the goal flash is a lightness/saturation change and Boost Up fires when the level increases, so validate them **live** rather than from a static frame.
* **Shared screenshots** — all boost meters, both in-game meters, and the paired save / epic save / demo / win meters each share one image.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

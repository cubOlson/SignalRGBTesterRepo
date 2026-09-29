# Among Us (3D)

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

## Effect Controls

Each group of events has its own on/off toggle in the SignalRGB UI: **Menu**, **Crew / Impostor** (role reveal), **Taskbar**, **Map**, **Meeting**, **Kill**, and **Victory / Defeat**.

The **Taskbar Effect** is a persistent HUD bar with its own color, position and size controls (TaskBar Color, TaskBar X / Y / Width / Height). Use **Adjustment Toggle** to draw the task bar full-size while out of game so you can position it on your devices, then turn it off before testing.

The background is driven by **Ambience brightness** (the screen-mirror ambience) and **Effect brightness** — these are ambience/level controls, not event effects.

---

## Testing Guidelines

The in-game gate is the white **players / P** button in the **top-right corner** (`pWhite`). Almost every in-match effect (taskbar, kill, vent, map, menu) checks it. Work through the states below roughly in the order of a match.

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter. The in-game HUD alone carries the gate, both task meters, the impostor indicator and the kill/vent meters (nine meters in one frame); the role-reveal, meeting, body-report, voting, victory and defeat screens each carry two or more.

### 1. Main menu  ·  `mainMenuBlue`

At the **AMONG US 3D** main menu, the **PLAY** button is highlighted blue. `mainMenuBlue` (≥ .95), with `pWhite` low (not in a match), triggers the **Menu** effect (floating crewmates + drifting stars).

![mainMenu](images/mainMenu.png)

### 2. Role reveal — Crewmate  ·  `crewmate`, `confirmCrewWhite`

At match start the screen shows **Your role is CREWMATE · Do your tasks · There are N Impostors among us**. `crewmate` (≥ .95, the cyan role text) with `confirmCrewWhite` (≥ .05, the white subtitle) sets the role to crew and plays the cyan **pulse**.

![crewmate](images/crewmate.png)

### 3. Role reveal — Impostor  ·  `impostor`, `confirmImpostorRed`

If you are the impostor the screen shows **Your role is IMPOSTOR · Kill and sabotage**. `impostor` (≥ .95, the red role text) with `confirmImpostorRed` (≥ .05, the red "kill/impostor" line) sets the role to impostor and plays the red **pulse**.

![impostor](images/impostor.png)

### 4. In game — HUD / Tasks / Kill / Vent  ·  `pWhite`, `totalTasks`, `firstTaskGreen`, `impostorRed`, `killWhite`, `killDarkGrey`, `killNotDarkGrey`, `ventRed`, `ventNotRed`

During a round the HUD shows: the **players / P** button top-right (`pWhite`, the gate), the task-progress bar + task list top-left (`totalTasks` reads the bar, `firstTaskGreen` confirms the first completed task; together they drive the **Taskbar** HUD), the impostor indicator (`impostorRed`, top-left role marker), and the **KILL** button bottom-right with its cooldown. A **kill** is read from `impostorRed` (≥ .95) + `killWhite` (≥ .95) + `killDarkGrey` (≥ .95) with `killNotDarkGrey` (< .1) → the you-killed effect; **vent** is read from `ventRed` (≥ .95) with `ventNotRed` (< .1) → the vent-grid effect. All of these share the one in-game frame.

![inGame](images/inGame.png)

### 5. Map open  ·  `mapWhite`, `mapBlue`, `mapRed`

Open the map. The map body reads blue for the crewmate/task map (`mapBlue` ≥ .95) or red for the sabotage/impostor map (`mapRed` ≥ .95), with `mapWhite` (< .1) guarding it. Triggers the **Map** effect (falling squares).

![map](images/map.png)

### 6. Emergency meeting  ·  `meetingWhite`, `meetingBlue`

Call a meeting. The **EMERGENCY MEETING** banner (white bar, red streaks, crewmate slamming the button) appears. `meetingWhite` (≥ .95, the far-right white edge of the banner) + `meetingBlue` (≥ .95, the blue button) trigger the **emergency-meeting** effect, which flows into voting.

![meeting](images/meeting.png)

### 7. Dead body reported  ·  `bodyFoundYellow`, `bodyConfirmWhite`, `bodyFoundColor`

When a body is reported, the report banner appears. `bodyFoundYellow` (≥ .95) + `bodyConfirmWhite` (≥ .95, the far-right white edge) trigger the **dead-body** effect, tinted by `bodyFoundColor` (a *colormean* meter that samples the reported crewmate's color). Then it flows into voting.

![bodyFound](images/bodyFound.png)

### 8. Voting  ·  `votingGrey`, `votingWhite`

After a meeting/report the voting screen shows the grey player panel. `votingGrey` and `votingWhite` read the panel; the **voting** effect runs while they are high and clears when both drop below ~.1 (which is when the ghost / dead effect can take over). *The reference frame shown here is the post-vote ghost state ("YOU'RE DEAD…"), where `votingGrey` has cleared (`_lt.1`).*

![voting](images/voting.png)

### 9. You were killed  ·  `deadRed`, `deadPink`

When an impostor kills you, the kill close-up flashes red/pink. `deadRed` (≥ .95) + `deadPink` (≥ .95) trigger the **getting-killed** effect.

![getKilled](images/getKilled.png)

### 10. Victory  ·  `victoryBlue`, `victoryDefeatWhite`

On a win, the blue **VICTORY** banner and white text fill the screen. `victoryBlue` (≥ .2) + `victoryDefeatWhite` (≥ .04) play the cyan **Victory** effect.

![victory](images/victory.png)

### 11. Defeat  ·  `defeatRed`, `victoryDefeatWhite`

On a loss, the red **DEFEAT** banner and white text fill the screen. `defeatRed` (≥ .2) + `victoryDefeatWhite` (≥ .04) play the red **Defeat** effect.

![defeat](images/defeat.png)

---

## Meter → Image map

| Image | Meters on it |
| --- | --- |
| `mainMenu.png` | `mainMenuBlue` |
| `crewmate.png` | `crewmate`, `confirmCrewWhite` |
| `impostor.png` | `impostor`, `confirmImpostorRed` |
| `inGame.png` | `pWhite`, `totalTasks`, `firstTaskGreen`, `impostorRed`, `killWhite`, `killDarkGrey`, `killNotDarkGrey`, `ventRed`, `ventNotRed` |
| `map.png` | `mapWhite`, `mapBlue`, `mapRed` |
| `meeting.png` | `meetingWhite`, `meetingBlue` |
| `bodyFound.png` | `bodyFoundYellow`, `bodyConfirmWhite`, `bodyFoundColor` |
| `voting.png` | `votingGrey`, `votingWhite` |
| `getKilled.png` | `deadRed`, `deadPink` |
| `victory.png` | `victoryBlue`, `victoryDefeatWhite` |
| `defeat.png` | `defeatRed`, `victoryDefeatWhite` |

That is **11 images for all 29 meters** — the in-game HUD groups nine meters into one frame.

---

## Notes

* **`pWhite` is the in-game gate** — the taskbar HUD, menu, kill and vent all check the top-right players/P button. The menu effect needs `pWhite` low (out of match).
* **`bodyFoundColor` is a colormean (HSV) meter** — it returns a hue/sat/value vector, so its test image uses the vector-component format (`_vx…` / `_vxy…`), not a scalar `_gte`. It samples the reported body's color to tint the effect.
* **Kill and vent are guarded pairs** — kill needs `killDarkGrey` high AND `killNotDarkGrey` low (plus `impostorRed` + `killWhite`); vent needs `ventRed` high AND `ventNotRed` low. A state is active only when the positive meters are high and the negative "Not" meter is low.
* **Meeting has two entry paths** — an **emergency meeting** (`meetingWhite` + `meetingBlue`) and a **dead-body report** (`bodyFoundYellow` + `bodyConfirmWhite`); both flow into **voting**, and when voting clears the **ghost / dead** effect can play.
* **Capture coverage** — the current test set (3840x2160 SDR) has one capture per state for 16 of the 29 meters. The remaining meters (`confirmImpostorRed`, `confirmCrewWhite`, `meetingWhite`, `bodyFoundColor`, `bodyConfirmWhite`, `deadPink`, `victoryDefeatWhite`, `killDarkGrey`, `killNotDarkGrey`, `votingWhite`, `mapWhite`, `mapBlue`, `ventRed`) live on the same screens shown above but still need their own crops captured, and other resolutions need the full set.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

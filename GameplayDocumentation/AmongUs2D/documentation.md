# Among Us (2D)

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Screen resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Each group of events has its own on/off toggle in the SignalRGB UI: **Menu**, **Crew / Impostor** (role reveal), **Taskbar**, **Map**, **Meeting**, **Kill**, and **Victory / Defeat**.

The **Taskbar Effect** is a persistent HUD bar with its own color, position and size controls (TaskBar Color, TaskBar X / Y / Width / Height). Use **Adjustment Toggle** to draw the task bar full-size while out of game so you can position it on your devices, then turn it off before testing.

The background is controlled separately by **Background Mode** (Ambience by default), **Background Brightness** and **Custom Color** — these are ambience, not event effects.

---

## Testing Guidelines

Two small HUD buttons in the **top-right corner** act as the context gate for almost everything: the **settings gear** (`SettingGray`) and the **map button** (`MapWhite`). Most in-match meters require `SettingGray` high; the map button distinguishes "in a room" (`MapWhite` low) from "map open" (`MapWhite` high). Work through the states below roughly in the order of a match.

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter. The in-game HUD, impostor HUD, map, dead-body report, voting, victory and defeat screens each carry multiple meters.

### 1. Main menu  ·  `mainMenuGreen`

At the main menu, the green "online / connected" dot sits next to your name (top-left, beside the **AMONG US** logo). `mainMenuGreen` (≥ .95) — with `SettingGray` and `MapWhite` both low — triggers the **Menu** effect (floating crewmates + drifting stars).

![mainMenu](images/mainMenu.png)

### 2. Role reveal — Crewmate  ·  `ChoseCrewmate`

At match start the cyan **CREWMATE** banner fills the center of the screen. `ChoseCrewmate` (≥ .95), guarded by `SettingGray` high and `MapWhite` low, sets the role to crew and plays the cyan **pulse**.

![crewmate](images/crewmate.png)

### 3. Role reveal — Impostor  ·  `ChoseImposter`

If you are the impostor the red **IMPOSTOR** banner fills the center instead. `ChoseImposter` (≥ .95), guarded by `VictoryDefeatWhite` low + `SettingGray` high + `MapWhite` low, sets the role to impostor and plays the red **pulse**.

![impostor](images/impostor.png)

### 4. In game + Task bar  ·  `SettingGray`, `MapWhite`, `TotalTasks`, `FirstTaskCompleted`

During a round the **TOTAL TASKS COMPLETED** bar runs across the top-left, and the gear + map buttons sit top-right. `SettingGray` (gear, ≥ .95) and `MapWhite` (map button) are the in-match gate. `TotalTasks` reads the width of the task-progress bar and drives the persistent **Taskbar** HUD; `FirstTaskCompleted` (≥ .95, the first green segment) confirms at least one task is done so the bar only draws once progress exists.

![inGame](images/inGame.png)

### 5. Impostor HUD — Kill / Report  ·  `KillButton`, `ImposterConfirm`, `ReportButton`

As the impostor the bottom-right shows the **KILL** (red skull), **SABOTAGE**, **VENT** and **SHIFT** buttons. `KillButton` reads the red kill button — a **kill** is detected when it *drops* from ready (`KillButton_gte.95`) to on-cooldown (`KillButton_lt.1`, a decrease of ≥ .5) while `ImposterConfirm` (≥ .95) and `ReportButton` (≥ .95) are present. Triggers the **Kill** (you-killed) effect. All these buttons are in the same bottom-right cluster, so **one screenshot covers them**.

![impostorHUD](images/impostorHUD.png)

### 6. Map open  ·  `MapWhite`, `MapBlue1`, `MapRed`

Open the map. The map button (`MapWhite`) goes high, and the map body reads blue for the crewmate/task map (`MapBlue1` ≥ .95) or red for the sabotage/impostor map (`MapRed` ≥ .95). Triggers the **Map** effect — falling squares, blue or red to match. One map screenshot covers the map meters.

![map](images/map.png)

### 7. Dead body reported / Emergency meeting  ·  `BodyFoundWhite`, `BodyFoundConfirm`, `BodyFoundColor`, `meetingBlue`

When a body is reported the **DEAD BODY REPORTED** banner (white bar, red streaks, skull bubble) appears; an emergency meeting shows a similar banner. `BodyFoundWhite` (≥ .95) reads the white banner. For a **report**, `BodyFoundConfirm` (≥ .95) is high and `meetingBlue` is low — the **dead-body** effect plays, tinted by `BodyFoundColor` (a *colormean* meter that samples the reported crewmate's color). For an **emergency meeting**, `meetingBlue` is high (== 1) and `BodyFoundConfirm` is low — the **emergency-meeting** effect plays. Both then lead into voting.

![report](images/report.png)

### 8. Voting / Dead  ·  `VotingGrey`, `deadRed`

The voting screen shows the grey player tablet. `VotingGrey` (≥ .95) reads the grey panel and drives the **voting** effect (crewmate cards sliding up). If **you** are dead, the red **DEAD** banner + red phone border are read by `deadRed` (≥ .3, while `BodyFoundWhite` is low and you are a crewmate) to play the **ghost / dead** effect. Both read from this meeting screen.

![voting](images/voting.png)

### 9. You were killed  ·  `GetKilledRed`

When an impostor kills you, the screen flashes red. `GetKilledRed` (≥ .95) — with `SettingGray` high and `MapWhite` ≥ .8 — plays the **getting-killed** effect (the kill animation).

![getKilled](images/getKilled.png)

### 10. Victory  ·  `VictoryBlue`, `VictoryDefeatWhite`

On a win, the blue **VICTORY** banner and white subtitle fill the screen. `VictoryBlue` (≥ .95) + `VictoryDefeatWhite` (≥ .95), with `SettingGray` and `MapWhite` both low (out of the match HUD), play the cyan **Victory** effect.

![victory](images/victory.png)

### 11. Defeat  ·  `DefeatRed`, `VictoryDefeatWhite`

On a loss, the red **DEFEAT** banner and white subtitle fill the screen. `DefeatRed` (≥ .95) + `VictoryDefeatWhite` (≥ .95), with `SettingGray` and `MapWhite` low, play the red **Defeat** effect.

![defeat](images/defeat.png)

---

## Meter → Image map

| Image | Meters on it |
| --- | --- |
| `mainMenu.png` | `mainMenuGreen` |
| `crewmate.png` | `ChoseCrewmate` |
| `impostor.png` | `ChoseImposter` |
| `inGame.png` | `SettingGray`, `MapWhite`, `TotalTasks`, `FirstTaskCompleted` |
| `impostorHUD.png` | `KillButton`, `ImposterConfirm`, `ReportButton` |
| `map.png` | `MapWhite`, `MapBlue1`, `MapRed` |
| `report.png` | `BodyFoundWhite`, `BodyFoundConfirm`, `BodyFoundColor`, `meetingBlue` |
| `voting.png` | `VotingGrey`, `deadRed` |
| `getKilled.png` | `GetKilledRed` |
| `victory.png` | `VictoryBlue`, `VictoryDefeatWhite` |
| `defeat.png` | `DefeatRed`, `VictoryDefeatWhite` |

That is **11 images for all 22 meters** — the HUD, impostor HUD, map, report, voting, victory and defeat screens each hold several meters.

---

## Notes

* **`SettingGray` + `MapWhite` are the context gate** — nearly every in-match meter checks the gear (`SettingGray` high) and uses the map button (`MapWhite`) to tell "in a room" (low) from "map open" (high). Victory / Defeat are the opposite: they require both low (you are out of the match HUD).
* **`BodyFoundColor` is a colormean (HSV) meter** — it returns a hue/sat/value vector, so its test images use the `_vxy<hue>,<sat>` vector-component format (e.g. `BodyFoundColor_vxy60,65.49.png`), not a scalar `_gte`. It samples the reported body's color to tint the effect.
* **The kill is a change-based meter** — `KillButton` fires on a *drop* (ready → cooldown, a decrease of ≥ .5), not a fixed level. Validate it live by performing a kill rather than from a single static frame.
* **Meeting has two paths** — a **report** (`BodyFoundConfirm` high, `meetingBlue` low) and an **emergency meeting** (`meetingBlue` high, `BodyFoundConfirm` low); both share the `BodyFoundWhite` banner and both flow into voting.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

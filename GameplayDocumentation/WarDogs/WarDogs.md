# War Dogs

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

The integration also carries aspect-ratio fallbacks, so other resolutions sharing those aspect ratios are read from the nearest matching profile.

---

## Resolution Setup

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the Display / Resolution option, then set the game to the resolution being tested. If it is not available, adjust the Aspect Ratio / Display Mode setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Kill**, **Downed**, **Healing**, **Revived**, **Building**, **Driving**, **Pilot**, **Low Ammo**, **Team Advance**, **Zone Contested**, **Win** and **Parachute**. **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

Most effects only fire while the integration considers you **in game** (the match HUD is visible — see step 2). The exceptions are **Driving** and **Pilot**, which read outside that state, and **Win**, which is read on the end-of-match victory card.

---

## A Note on Testing War Dogs

War Dogs matches are **long** — often over an hour — and a match only ends when a team pushes ~100 soldiers into the zone. So the meters are split across two contexts:

* **Firing range** — the fastest way to trigger the loadout/vehicle states on demand (Low Ammo, Driving, Pilot, Parachute, Building) without waiting on a match.
* **Live match** — the states that only happen in real play (In game, Kill, Team Advance, Zone Contested, Downed, Revived, Healing, Win).

**One screen can drive several meters**, so you do not need a separate capture for every meter — a single image (e.g. the Kill banner) validates all the meters that read that screen.

> **Before you queue for a match**, open your loadout and add a **healing item (bandages)**. You need it in your kit to trigger the Healing meters later (step 8).

---

## Testing Guidelines

### 1. Firing range — loadout & vehicle states

Enter the **firing range** to trigger these on demand:

* **Low Ammo** — fire until the magazine runs low so the red low-ammo warning shows: `lowAmmoRed` (positive) goes high while `lowAmmoNotRed` (negative) stays low.

![low ammo](images/low_ammo.png)

* **Building** — start constructing (hammer/build tool). `buildingWhite1` and `buildingWhite2` read the build UI while `buildingNotWhite` guards the region.

![building](images/building.png)

* **Driving** — get into a ground vehicle. `drivingWhiteTank` reads the vehicle icon, guarded by `drivingNotWhite`.

![driving](images/driving.png)

* **Pilot** — get into an aircraft and fly. `pilotWhiteTank` reads the pilot icon, guarded by `pilotNotWhite`.

![pilot](images/pilot.png)

* **Parachute** — deploy/redeploy so the parachute prompt appears. `parachuteGreyBar` reads the grey deploy bar and `parachuteWhiteConfirm` the confirm, guarded by `parachuteNotGrey`.

![parachute](images/parachute.png)

### 2. Enter a match — in game

Queue into a normal match. Once the match HUD is visible, the in-game state activates and most other effects unlock.

The colored HUD markers trigger `inGameBlue`, `inGameGreen` and `inGameRed` together. **Most effects below only fire while this is active.**

![ingame](images/ingame.png)

### 3. Kill an enemy

Get a kill during the match. The kill banner triggers `killWhite1` and `killWhite2`, guarded by `killNotWhite`.

![kill](images/kill.png)

### 4. Team advance

As the fight moves, the team-advance prompts appear when a team pushes the line. Each faction color is its own meter: `teamAdvanceRed`, `teamAdvanceGreen` and `teamAdvanceBlue`. You will pick these up naturally during play.

![teamadvance](images/teamadvance.png)

### 5. Zone contested

While a point is being fought over, the zone-contested banner shows. `zoneContestedBar` reads the contest bar and `zoneContestedWhiteText` reads the banner text. Also picked up naturally during play.

![zone contested](images/zone_contested.png)

### 6. Get downed

Let yourself be **downed** — and do it **near teammates**, so someone can pick you up (needed for step 7). The downed HUD triggers `downedWhite1`, `downedWhite2` and `downedRed` together.

**Downed has priority:** it clears the screen and blocks the other effects while active, until the red clears or you leave the match.

![downed](images/downed.png)

### 7. Revived

Have a teammate revive you from the downed state. The green revive cue triggers `revivedGreen` and `revivedGreen2`.

![revived](images/revived.png)

### 8. Healing

With a **healing item (bandages) in your loadout** (see the note above), take a little damage — or just after being revived — and use the bandage. The heal cue triggers `healingWhite`, guarded by `healingNotWhite`; the `healingWhiteBar` meter then drives the fill level of the Healing effect.

![healing](images/healing.png)

### 9. Win

The **Win** meters only appear on the victory card, and matches run long (often over an hour, ending when a team pushes ~100 soldiers into the zone). To catch a win efficiently:

* Look for matches that are **already past the hour mark** — they are closer to ending.
* **Hop in and out of matches** to find one near its finish, so you are present when the victory card appears.

On the card, the winning faction is chosen by whichever team-color meter reads high — `winTeamRed`, `winTeamGreen` or `winTeamBlue` — and the effect fires once the white **WINS** text is present: `winWhiteText` (positive), guarded by `winNotWhiteText`.

![win](images/win.png)

---

## Notes

* **In-game detection gates most effects** — Kill, Building, Downed, Healing, Revived, Low Ammo, Team Advance, Zone Contested and Parachute all require the in-game state (step 2). **Driving** and **Pilot** read outside it (use the firing range), and **Win** is read on the victory card.
* **Downed and Revived have priority** — while either is active it clears the screen and blocks the other effects, so trigger them deliberately rather than mid-sequence.
* **Positive / negative pairs** — most states use a positive meter plus a `…Not…` negative guard (e.g. `killWhite1` + `killNotWhite`). Both must read correctly for the effect to fire cleanly.
* **One screen validates several meters** — you do not need a separate capture per meter; a single image covers every meter that reads that screen.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

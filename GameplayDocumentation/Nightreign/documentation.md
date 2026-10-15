# Elden Ring Nightreign

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

Change the monitor resolution, launch the game and open the display/graphics settings.

Verify that the selected resolution appears in the **Screen Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Aspect Ratio** / display mode so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Player Color Effect**, **Yellow Health Effect**, **Player / Ally Downed Effect**, **Boss Effect** and **Victory / Defeat Effect**. **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

Most effects only fire while the integration considers you **in game** (see step 1). The exception is **Victory / Defeat**, which reads the end-of-run banners while the compass is hidden.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

Test images live in `SDR/<meter>/<meter>_<op><value>.png`.

### 1. In game (gate)  ·  `inGameWhite`, `inGameRed`, `inGameRune`, `inGameNotWhite`, `inGameNotRune`

The in-game state needs the level badge (top-left) **and** the rune counter (top-right):

* the badge ring reads white **or** red (`inGameWhite` >= .95 **or** `inGameRed` >= .95), guarded by `inGameNotWhite` <= .05, and
* the rune counter reads (`inGameRune` >= .95), guarded by `inGameNotRune` <= .05.

It turns off when the white, red and rune meters all drop to <= .05. **Most effects require this state.** `inGameRed`, `inGameRune`, `inGameNotWhite` and `inGameNotRune` use the same screenshot.

Red badge + rune counter:

![in game](images/in_game.png)

White badge:

![in game white](images/in_game_white.png)

### 2. Compass  ·  `compassWhite`

The compass diamond at the top-center: `compassWhite` >= .95. The compass is a **context meter** with no effect of its own: it must be **hidden** (<= .05) on the Victory and Defeat banners, and it reappearing (>= .95) ends the Boss effect.

![compass](images/compass.png)

### 3. Player Color  ·  `playerColor`

The colored strip on the left edge of the HUD (next to the level badge) is the player's color. When the in-game state turns on, `playerColor` reads its **hue** and plays the **Player Color** effect: a band of that color expands from the center with a white marker, then fades. It plays once per time you enter the game. The strip is visible at the left of the badge in the compass screenshot above.

### 4. Yellow Health  ·  `yellowHit`, `notYellowHit`

The yellow chip-damage segment on the health bar (top-left): `yellowHit` >= .05 with `notYellowHit` <= .05. Plays the **Yellow Health** effect (a flash that shifts red to yellow, then collapses).

![yellow hit](images/yellow_hit.png)

### 5. Player Downed  ·  `playerDowned`, `playerNotDowned`

The blue revive ring over your downed character: `playerDowned` >= .95. Plays the **Player Downed** effect (the screen darkens and blue orbs circle the center). It ends once the ring clears (`playerDowned` <= .05).

![player downed](images/player_downed.png)

### 6. Ally 1 Downed  ·  `ally1Downed`, `ally1NotDowned`

The blue revive ring over the first teammate's portrait (top-left, under your HUD): `ally1Downed` >= .95. Plays the **Ally Downed** effect (fast blue orbs circle the center). It ends once neither ally is downed.

![ally 1 downed](images/ally1_downed.png)

### 7. Ally 2 Downed  ·  `ally2Downed`, `ally2NotDowned`

The same ring over the second teammate's portrait: `ally2Downed` >= .95. Plays the same **Ally Downed** effect.

![ally 2 downed](images/ally2_downed.png)

### 8. Boss  ·  `bossRed`, `bossNotRed`

The red boss health bar at the bottom-center: `bossRed` >= .25 with `bossNotRed` <= .05. Plays the **Boss** effect: a red pulse over the keyboard whose color and speed follow the remaining boss health. It ends when the bar is gone (`bossRed` <= .05) and the compass is back.

![boss](images/boss.png)

### 9. Defeat  ·  `defeatRed`, `defeatNotRed`

The red **DEFEAT** banner in the center of the screen: `defeatRed` >= .15 with `defeatNotRed` <= .05, while the rune counter is still visible (`inGameRune` >= .95) and the compass is hidden (<= .05). Plays the **Defeat** effect (a red stripe expands from the center and fades). It ends when the banner is gone.

![defeat](images/defeat.png)

### 10. Victory  ·  `winWhite`, `winNotWhite`

The **NIGHT REPULSED** banner in the center of the screen: `winWhite` >= .15 with `winNotWhite` <= .05, under the same conditions as Defeat (rune counter visible, compass hidden — it is replaced by a diamond icon on this screen). Plays the **Victory** effect (the same stripe as Defeat in blue). Both meters use the same screenshot.

![victory](images/victory.jpg)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (badge + rune counter) | `inGameWhite`, `inGameRed`, `inGameNotWhite`, `inGameRune`, `inGameNotRune` |
| Compass (context) | `compassWhite` |
| Player color strip | `playerColor` |
| Yellow health | `yellowHit`, `notYellowHit` |
| Player downed | `playerDowned`, `playerNotDowned` |
| Ally 1 downed | `ally1Downed`, `ally1NotDowned` |
| Ally 2 downed | `ally2Downed`, `ally2NotDowned` |
| Boss health bar | `bossRed`, `bossNotRed` |
| Defeat banner | `defeatRed`, `defeatNotRed` |
| Victory banner | `winWhite`, `winNotWhite` |

**21 meters across 10 screens** driving 7 effects.

---

## Notes

* **In-game detection gates most effects.** Victory and Defeat only fire while the compass is hidden and the rune counter is still visible, so the banners must be on screen over the normal HUD.
* **Positive / negative pairs** — most states use a positive meter plus a `…Not…` negative guard (e.g. `bossRed` + `bossNotRed`). Both must read correctly for the effect to fire cleanly.
* **Player Color plays once per entry** — it re-arms only after the in-game state turns off, so leave the run and re-enter to retest it.
* **Downed effects follow the HUD** — they hold for as long as the ring is visible and clear when it disappears or you leave the run, so validate them **live**.
* **Shared screenshots** — several meters read the same screen and share one image (for example the four `inGame…` meters and the `…Downed` / `…NotDowned` pairs).
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

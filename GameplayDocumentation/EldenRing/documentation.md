# Elden Ring

## Supported Resolutions

* 1920x1080
* 2560x1080
* 2560x1440
* 2560x1600
* 3440x1440
* 3840x1080
* 3840x2160
* 3840x2160 (HDR)

The integration also carries aspect-ratio fallbacks (**3.55:1**, **24:10** and **16:11**), so other resolutions sharing those aspect ratios are read from the nearest matching profile.


---

## Resolution Setup

Change the monitor resolution, launch the game and open **System → Graphics**.

Verify that the selected resolution appears in the **Screen Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Screen Mode** so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Health Bar**, **Mana Effects**, **Stamina Effects**, **Boss Health Effect**, **Death Effect**, **Boss Kill Effect**, **Discovered Grace Effect**, **Rune Gain Effect** and **Great Rune Effect**. The HUD bars have position/size sliders, an **Adjustment toggle** (turn it off in game) and a **Vertical Bars** option, and **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

All effects only fire while the integration considers you **in game** (see step 1), except **Death** and **Boss Kill**, which fire on full-screen banners and clear any running effects.

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter.

### 1. In game (gate)  ·  `compass`, `hpRed`, `HpYellow`, `HpNotYellow`

The in-game state turns on when the **compass** at the top of the screen reads (`compass` ≥ .95) with the HP bar present (`hpRed` > .05), or when the HP bar length reads (`HpYellow` ≥ .1, guarded by `HpNotYellow` ≤ .05). It turns off when the compass is gone (`compass` ≤ .05) or the HP bar is gone. **Most effects require this state.**

![in game](images/in_game.png)

### 2. Health, Mana and Stamina bars  ·  `hpRed`, `HpYellow`, `HpNotYellow`, `manaBlue`, `manaYellow`, `manaNotYellow`, `staminaGreen`, `staminaYellow`, `staminaNotYellow`

The three bars at the top left. The bar **color** (`hpRed`, `manaBlue`, `staminaGreen`) confirms the bar is present; the **yellow** end-cap region (`…Yellow`, guarded by `…NotYellow` ≤ .1) measures its length. The length is compared against the maximum to fill the matching bar on the keyboard.

Test with all three bars visible, and again after the bars have been extended (level up / equipment).

![hud bars](images/hud_bars.png)

### 3. Boss Health  ·  `bossHpRed`, `bossHpYellow`, `bossHpNotYellow`

The **boss health bar** at the bottom of the screen while you are fighting a boss or a named enemy. `bossHpRed` confirms the bar is present, `bossHpYellow` measures its length, guarded by `bossHpNotYellow`.

![boss health](images/boss_health.png)

### 4. Death  ·  `diedRed`, `diedNotRed1`, `diedNotRed2`

The **YOU DIED** banner: `diedRed` > .2 with your HP empty (`hpRed` < .01), guarded by `diedNotRed1` and `diedNotRed2` < .05. Clears all running effects and plays the **Death** effect (expanding dark-red rings with rising white particles).

![death](images/death.png)

### 5. Boss Kill  ·  `bossKillYellow`, `bossKillNotYellow1`, `bossKillNotYellow2`

The **ENEMY FELLED** (or similar) banner: `bossKillYellow` ≥ .2, guarded by `bossKillNotYellow1` and `bossKillNotYellow2` ≤ .05. Clears all running effects and plays the **Boss Kill** effect (blue / cyan / white particles rising for about 5 seconds).

![boss kill](images/boss_kill.png)

### 6. Discovered Grace  ·  `discoverOrange`, `discoverNotOrange1`, `discoverNotOrange2`

The orange **LOST GRACE DISCOVERED** banner: `discoverOrange` ≥ .2, guarded by `discoverNotOrange1` and `discoverNotOrange2` ≤ .05. Plays the **Discovered Grace** effect (an amber wash with expanding gold rings). It re-arms once the banner is gone (`discoverOrange` ≤ .05).

![discovery](images/discovery.png)

### 7. Rune Gain  ·  `runeWhite`, `runeNotWhite`

The **rune counter** at the bottom right. It is change-based: the effect fires when `runeWhite` ≥ .1 and the meter changes by more than .05, guarded by `runeNotWhite` ≤ .05. Plays the **Rune Gain** effect (a pale gold chevron sweeping up the screen).

![rune](images/rune.png)

### 8. Great Rune  ·  `greatRuneBright`, `greatRuneDark`

The **Great Rune** icon at the top left. It is change-based: the effect fires when the icon goes from dark to bright (`greatRuneBright` ≥ .1, `greatRuneDark` ≤ .05, both changing by more than .25), which is when you activate the rune. It re-arms when the icon is dark again (`greatRuneDark` ≥ .95 and `greatRuneBright` ≤ .05). Plays the **Great Rune** effect (a yellow ring expanding from the center with yellow sparks).

![great rune](images/great_rune.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate (compass + HP bar) | `compass`, `hpRed`, `HpYellow`, `HpNotYellow` |
| Mana bar | `manaBlue`, `manaYellow`, `manaNotYellow` |
| Stamina bar | `staminaGreen`, `staminaYellow`, `staminaNotYellow` |
| Boss health bar | `bossHpRed`, `bossHpYellow`, `bossHpNotYellow` |
| YOU DIED banner | `diedRed`, `diedNotRed1`, `diedNotRed2` |
| ENEMY FELLED banner | `bossKillYellow`, `bossKillNotYellow1`, `bossKillNotYellow2` |
| LOST GRACE DISCOVERED banner | `discoverOrange`, `discoverNotOrange1`, `discoverNotOrange2` |
| Rune counter | `runeWhite`, `runeNotWhite` |
| Great Rune icon | `greatRuneBright`, `greatRuneDark` |

**26 meters across 9 screens** driving 5 effects (Death, Boss Kill, Discovered Grace, Rune Gain, Great Rune) plus the live Health, Mana, Stamina and Boss Health bars.

---

## Notes

* **Death and Boss Kill clear the screen** — they reset the running effects when they start, so trigger them deliberately rather than mid-sequence.
* **Banner effects are one-shot** — Death, Boss Kill and Discovered Grace play once per banner and re-arm when the banner disappears.
* **Rune Gain and Great Rune are change-based** — validate them **live**, not just from a static frame.
* **Positive / negative pairs** — most states use a positive meter plus a `…Not…` negative guard (e.g. `diedRed` + `diedNotRed1`/`diedNotRed2`). Both must read correctly for the effect to fire cleanly.
* **Death, Boss Kill and Discovered Grace share one region** — `diedRed`, `bossKillYellow` and `discoverOrange` read the same banner area and differ only by color.
* **Boss health guard name mismatch** — the script reads `engine.vision.bossHPNotYellow`, but the meter is defined as `bossHpNotYellow`. With the capital `P` the guard is `undefined`, so the boss health length may never update. Verify this live and fix the spelling if the boss bar does not move.
* **Great Rune test images** — `greatRuneBright_gte.1.png` and `greatRuneDark_lte.05.png` are currently identical files, so one of them does not show the state its name describes. Re-capture the dark (inactive) one.
* **Capture status** — 26 test images captured at 3840x2160 SDR only. Other resolutions and HDR are not captured yet.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

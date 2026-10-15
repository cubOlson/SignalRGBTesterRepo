# Monster Hunter Wilds

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

Change the monitor resolution, launch the game and open the graphics settings.

Verify that the selected resolution appears in the Display / Screen Resolution option, then set the game to the resolution being tested. If it is not available, adjust the Display Mode setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

**Keep the HUD on its default layout.** The meters read fixed regions: the health, stamina and sharpness bars in the upper-left, the Palico health bar under them, and the quest timer clock in the upper-right. Do not hide or rescale these elements.

---

## Effect Controls

Each effect has its own on/off toggle in the SignalRGB UI: **Health HUD Effect**, **Stamina HUD Effect**, **Red Health Effect**, **Low Health Effect**, **Low Stamina Effect**, **Palico Low Health Effect**, **Weapon Sharpness Effect** and **Timed Quest Effect**. The remaining controls are:

* **Health HUD** and **Stamina HUD** — two persistent bars that mirror your health and stamina on the devices. Each one has its own **Color**, **Horizontal**, **Vertical**, **Width** and **Height** controls.
* **HUD Adjust Toggle (turn off in-game)** — draws both bars at full size while you are out of the game, so you can position them before testing. **Turn it off before you test in a hunt.**
* **Background Mode** — the backdrop (Ambience, Color Cycle, Custom, or any of the Audio Visualizer variants), with **Background Brightness** and **Custom Color**.

---

## A Note on Testing Monster Hunter Wilds

Most of the effects only work during a **quest or an expedition**, and many only fire while the integration considers you **in combat**. To test efficiently:

* **Pick a quest against a monster you can fight comfortably**, so you can control when you take damage and when you spend stamina.
* **A Timed Quest** (any quest with the clock in the upper-right) is needed for the timer effect.
* **Use a melee weapon** for the sharpness effect. The sharpness gauge does not exist on Bowguns, as can be seen on the Bowgun frames.

**One screen can drive several meters**, so you do not need a separate capture per meter. A single image of the upper-left bars validates the health, stamina and sharpness meters that read them.

> **Before you start**, bring a **Whetstone** (to change the sharpness gauge), and a way to hurt yourself on purpose, such as standing in front of a monster. Also bring the **Palico**, because the Palico meter needs its health bar on screen.

---

## Testing Guidelines

### 1. Start a quest — in game

Accept a quest and wait until the HUD is visible. The in-game state activates when `inGame01` or `inGame02` reads high while `inGameNot` stays low. The Health and Stamina HUD bars start following your bars as soon as health and stamina read above their minimums. **Every effect below requires this state.**

![ingame](images/ingame.png)

### 2. Enter combat

Approach a monster until the fight starts. `inCombat01` and `inCombat02` read high while `inCombatNot` stays low. Most effects only fire while combat is active, so they stay off in the camp or while travelling. This frame is the same one used for in-game, and a single image validates both.

![combat](images/combat.png)

### 3. Health and stamina HUD

The five `health1` … `health5` meters read the green health bar, one segment each, because the bar grows longer with your max health. `stamina1` … `stamina3` read the yellow stamina bar the same way. The two HUD bars on the devices follow them, and they fade out when you leave the hunt.

![health](images/health.png)

### 4. Take damage — red health

Get hit by the monster. The part of the health bar that can still be recovered turns red, which `healthRed` reads (≥ 0.1, and it must change by at least 0.05 at once). Red drops fall across the devices. It stops when the red part is gone, or when you leave combat.

![red health](images/red_health.png)

### 5. Low health

Keep taking damage until only the very start of the health bar is left. The effect fires when `health1` still reads about 0.2 while `health2` is empty. A pulsing green-to-red glow spreads from the edges of the devices and stops when your health recovers. *(Check this threshold on your own bar, because it depends on the max health of your build.)*

### 6. Low stamina

Run, dodge or attack until the yellow stamina bar is almost empty. The effect fires when `stamina1` still has a sliver (≥ 0.05) while `stamina2` is empty, as long as you are not almost dead. A yellow and red pulse spreads from the edges of the devices, and stops when the stamina recovers to `stamina1` ≥ 0.2.

![stamina](images/stamina.png)

### 7. Palico low health

Let your Palico take damage until its health bar (under yours, with the Palico's name) is empty. `palicoLow` reads ≤ 0.01 and red hearts pulse across the devices every moment. It stops when the Palico recovers (≥ 0.25) or when you leave the combat.

![palico](images/palico.png)

### 8. Weapon sharpness

With a melee weapon, use your **Whetstone** (or attack until the sharpness drops a color). The sharpness gauge sits under the stamina bar. `sharpConfirm` confirms the gauge is shown and `sharpLength` reads its length (≥ 0.15). When the length changes by 0.2 or more, or the color changes by at least 30 degrees of hue, a blade sweep in the new sharpness color plays across the devices.

![sharpness](images/sharpness.png)

### 9. Timed quest

On a timed quest, the clock sits in the upper-right corner. `timerConfirmRed` and `timerConfirmGrey` confirm the clock is on screen. The clock hand then passes four marks, read by `timeYellow1` to `timeYellow4`, and a yellow clock sweep plays on the devices at each quarter (a quarter turn further each time). Each mark triggers only once.

![timer confirm](images/timer_red.png)

![timer mark](images/timer_yellow.png)

---

## Notes

* **In-game and in-combat gate the effects** — Red Health, Low Health, Low Stamina and Palico need both states. Weapon sharpness only needs in-game, and the HUD bars only need in-game. If nothing triggers, check the HUD layout first.
* **Low Health and Low Stamina read bar segments** — the effects need a partly filled first segment, so a bar that jumps straight to empty may not trigger them. Lower it gradually.
* **Sharpness needs a melee weapon** — Bowguns do not have the gauge.
* **Timer needs a timed quest** — in an expedition without the clock, the timer meters never match.
* **Same frame, several meters** — for example the `inGame01`, `inCombat01`, `health1` and `stamina1` frames all show the whole upper-left HUD.
* **Screenshot frames in this folder** — the QA screenshots in `SDR/` are organized per meter, and the suffix of each file name is the match threshold used by the meter (for example `inGame01_gte.95` = at least 0.95, `inCombatNot_lt.1` = below 0.1). `sharpColor` frames are named after the color vector values instead. Only 3840x2160 SDR frames are included, so other resolutions still need their own captures.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

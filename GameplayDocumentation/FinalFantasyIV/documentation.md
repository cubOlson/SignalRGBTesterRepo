# Final Fantasy XIV

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

Change the monitor resolution, launch the game and open **System Configuration → Graphics Settings**.

Verify that the selected resolution appears in the **Screen Size / Resolution** option, then set the game to the resolution being tested. If it is not available, adjust the **Screen Mode** so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Effect Controls

Every effect below has its own on/off toggle in the SignalRGB UI: **Health effect**, **Mana effect**, **Level Up effect**, **Target effect** and **Attacking effect**. The Health and Mana HUD bars have an **enable HUD** toggle plus position/size sliders, and **Background Mode** selects how the backdrop reacts (ambience, color cycle, audio or custom).

All effects only fire while the integration considers you **in game** (see step 1).

---

## Testing Guidelines

> **One screenshot can cover several meters.** Where several meters read the same screen they share a single image — you do **not** need one screenshot per meter. Meters that are change-based (HP, MP, Level) need two images, one for each state.

### 1. In game (gate)  ·  `inGameNRed`, `inGameDownRed`, `inGameNotRed`

Load into the world with the normal HUD visible. The in-game state turns on when `inGameNRed` ≥ .95 **and** `inGameDownRed` ≥ .95 with `inGameNotRed` < .1. **All effects require this state.**

![in game](images/in_game.png)

### 2. Health  ·  `hp`

The **HP bar** at the bottom of the screen. The effect is change-based: it fires when the bar changes by more than .04 (after it settles for a few frames).

* **Health Increase** — HP rises (heal, regen, potion).
* **Health Decrease** — HP drops (taking damage).

Test with a full bar (`hp_gte.95`) and a lowered bar (`hp_lt.95`).

![hp](images/hp.png)

### 3. Mana  ·  `mp`

The **MP bar** next to the HP bar. Also change-based.

* **Mana Increase** — MP rises by more than .03 (regen, MP restore).
* **Mana Decrease** — MP drops by more than .04 (casting a spell).

Test with a full bar (`mp_gte.95`) and a lowered bar (`mp_lt.95`).

![mp](images/mp.png)

### 4. Level Up  ·  `level`

The **experience bar** along the bottom of the screen. When you level up the bar empties, so the effect fires when the meter *decreases* by more than .5. Test with a full bar (`level_gte.5`) and an emptied bar (`level_lt.5`).

![level](images/level.png)

### 5. Target  ·  `target`, `targetNot`

The **target bar** at the top of the screen appears when you select an enemy: `target` ≥ .75, guarded by `targetNot` < .1. A lingering effect that holds while you have a target and ends when it is dropped (`target` < .1).

![target](images/target.png)

### 6. Attacking  ·  `attack`

The **auto-attack indicator** in the middle of the screen: `attack` ≥ .5. A lingering effect that holds while you are attacking and ends when `attack` < .1.

![attack](images/attack.png)

---

## Meter → Screen map

| Screen | Meters |
| --- | --- |
| In-game gate | `inGameNRed`, `inGameDownRed`, `inGameNotRed` |
| Health bar (Health Increase / Decrease) | `hp` |
| Mana bar (Mana Increase / Decrease) | `mp` |
| Experience bar (Level Up) | `level` |
| Target bar (Target) | `target`, `targetNot` |
| Auto-attack indicator (Attacking) | `attack` |

**9 meters across 6 screens** driving 7 effects.

---

## Notes

* **In-game detection gates every effect** — without the gate, no effect fires. The Health and Mana HUD bars show at full size out of game.
* **HP, MP and Level are change-based** — they fire on the *change* of the meter (after it settles for 5 samples), not on a static frame. Validate them **live** and with the paired `_gte` / `_lt` images.
* **Target and Attacking are lingering** — they stay on while their meter reads high and clear when it drops below .1.
* **Positive / negative pairs** — the gate and Target use a `…Not…` negative guard (`inGameNotRed`, `targetNot`). Both must read correctly for the effect to fire cleanly.
* **Capture status** — 12 test images captured at 3840x2160 SDR only. See `meters.txt` / `FinalFantasy_test_images.txt`. Other resolutions and HDR are not captured yet.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

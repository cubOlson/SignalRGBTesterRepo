# Project Zomboid

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

## Testing Guidelines

Most effects only fire while the integration considers you **in game** — the play/pause time controls in the **top-right corner** are used as the "playing" gate (`inGameRed` reads the red play triangle, `inGameWhite` reads the white pause/fast-forward icons). Load a save and spawn into the world first, then trigger the individual states below.


### 1. In game  ·  `inGameRed`, `inGameWhite`

Spawn into the world. In the **top-right corner** the time controls are visible — the red **play** triangle and the white **pause / fast-forward** icons.

The red play triangle drives `inGameRed` (positive, fires at ≥ 0.02) and the white icons drive `inGameWhite` (positive, fires at ≥ 0.15). Together they set the in-game state that gates every effect except Death. Both meters read the **same top-right region**, so this one screenshot validates both.

![inGame](images/inGame.png)

### 2. Inventory  ·  `inventoryBrown`

Open your inventory / a loot container. `inventoryBrown` reads the brown inventory button on the left toolbar and fires at ≥ 0.95. → **Inventory Effect** (blinking slot grid).

![inventory](images/inventory.png)

### 3. Health  ·  `healthPink`

Open the health panel (the heart icon on the left toolbar). `healthPink` reads the pink health button and fires at ≥ 0.95. → **Health Effect** (body scan, limbs blink red).

![health](images/health.png)

### 4. Craft  ·  `craftBlue`

Open the crafting window. `craftBlue` reads the blue crafting region on the left toolbar and fires at ≥ 0.95. → **Craft Effect** (hammer drives a nail).

![craft](images/craft.png)

### 5. Build  ·  `buildBrown`

Open the build menu. `buildBrown` reads the brown build button and fires at ≥ 0.5. → **Build Effect** (brick wall stacks up).

![build](images/build.png)

### 6. Move  ·  `moveBrown`

Enter move-furniture mode. `moveBrown` reads the brown move button and fires at ≥ 0.95. → **Move Effect** (4-way move cursor drifts).

![move](images/move.png)

### 7. Search  ·  `searchBlue`

Enter search / investigate-loot mode (the magnifying-glass button). `searchBlue` reads the blue search button and fires at ≥ 0.95. → **Search Effect** (magnifying glass sweeps).

![search](images/search.png)

### 8. Animals  ·  `animalsBlue`

Open the animals panel. `animalsBlue` reads the blue animals button and fires at ≥ 0.95. → **Animals Effect** (pasture grows, animals appear).

![animals](images/animals.png)

### 9. Weapon  ·  `weapon1Black`, `weapon2Black`

Equip / swap a weapon. The two weapon slots sit at the **top-left** of the toolbar. `weapon1Black` (slot 1) and `weapon2Black` (slot 2) both read the dark slot background; the Weapon Effect fires on the *change* — when either slot's black reading **drops** by more than 0.2 (a weapon icon fills the dark slot). Both slots are in the same top-left area, so **one screenshot covers both meters**.

![weapon](images/weapon.png)

### 10. Damage  ·  `damageRed`

Take a hit (a zombie scratch/bite, or fall/environment damage). The moodle / status icons run down the **right edge** of the screen. `damageRed` reads a tall red strip on the right edge and fires on an **increase** greater than 0.05 (a fresh injury pushing the red up). → **Damage Effect** (red paint splash).

![damage](images/damage.png)

### 11. Car  ·  `carWhite`, `carGrey`

Get into a vehicle and start driving. The dashboard appears at the **bottom-center**. `carWhite` reads the white dashboard text/gauges and `carGrey` reads the grey dashboard panel beside it — the Car Effect fires only when **both** are high (≥ 0.95) and holds while you stay in the vehicle. Both meters read from the same dashboard, so **one screenshot covers both**.

![car](images/car.png)

### 12. Death  ·  `deathRed`, `deathWhiteText`

Let a zombie kill you (or die from injury). The **death summary** screen appears ("You survived for … / You killed … zombies"). **This state is read even when the in-game gate is off**, so it is the one effect not gated by `inGame`. `deathRed` reads the red accent (the zombie moodle, top-right, fires at ≥ 0.1) and `deathWhiteText` reads the white summary text across the center (fires at ≥ 0.15). Both must read high together to fire the Death Effect (zombie head in a red ring, blood rain). This one screenshot covers both meters.

![death](images/death.png)

---

## Notes

* **In-game detection gates most effects** — inventory, health, craft, build, move, search, animals, weapon, damage and car all require the in-game state (`inGameRed` + `inGameWhite`). **Death is read on its own screen** and fires regardless of the gate.
* **Change-based meters** — `weapon1Black` / `weapon2Black` fire on a *drop* (slot goes from empty-dark to holding an icon) and `damageRed` fires on an *increase* (fresh injury). These react to a change, not a fixed level, so validate them by triggering the event live rather than from a static frame alone.
* **Paired meters share a screenshot** — In game, Weapon, Car and Death each read two meters from one region, so capture one screenshot per pair. The seven left-toolbar panels are one meter each and only highlight while their own panel is open, so each needs its own screenshot.
* Detection strategy: each positive meter measures the percentage of its small region matching an HSL color range and fires above the threshold shown above. If an effect never fires, load a target-resolution screenshot in the QA Tool and verify the meter region captures the right pixels at the right color, then adjust the HSL range or coordinates.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA Tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

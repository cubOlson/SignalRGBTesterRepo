# Mortal Shell 2

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

Every effect below has its own on/off toggle in the SignalRGB UI (Death, Rest, Item, Coin, Gloom, Boss, Low Health, Heal, Resolve, Guard, Damage, Inventory). The **Health Bar Effect** and **Resolve Bar Effect** are persistent HUD bars with their own color/position/size controls. Use **Bar Effect Adjust** to draw the health and Resolve bars at full size while out of game so you can position them before testing.

---

## Testing Guidelines

Follow the steps in order — this is the natural sequence of play, and each step triggers its meters as you go.

### 1. Enter the game

As soon as you load into gameplay and the HUD is visible, several meters activate immediately:

* **In-game detection** — the top-right black marker (`ingameBlack`) plus its paired "not black" region (`inGameNotBlack`) and the health-bar confirmation pixel (`healthConfirmRed`) together confirm the in-game state. Most other effects only fire while this is active.
* **Health bar** — `healthBarRed` (bar fill) and `healthConfirmRed` (confirmation pixel).
* **Resolve bar** — the yellow bar just above the health bar: `resolveBarYellow` (bar fill) and `resolveConfirmYellow` (confirmation pixel).
* **Coins** — the currency counter in the top-right (`coinsNumber`).
* **Gloom** — the Gloom counter in the top-right (`gloomNumber`).

![ingame](images/ingame.png)

### 2. Fight an enemy

Engage an enemy to exercise the combat meters:

* **Guard** — press **Alt** to Harden while fighting. The gold guard bar fills in the lower-center of the screen (`guardBar`) and triggers the **Guard** effect.
* **Damage** — take a hit. The sharp drop in `healthBarRed` triggers the **Damage** effect.
* **Low Health** — take enough damage that your health falls to a sliver (`healthBarRed` at or below ~0.2) to trigger the **Low Health** heartbeat effect.

![guard](images/guard.png)

### 3. Die

Let the enemy kill you. The death screen shows the black vignette in the center — `deathBlack1` and `deathBlack2` read high while `deathNotBlack` reads low — triggering the **Death** effect.

![death](images/death.png)

### 4. Rest at the beacon

After dying you respawn at the beacon. Press **E** to **Rest at Beacon**; the `restBeacon` meter reads the prompt text (paired with `restBeaconNot` for the baseline) and triggers the **Rest** effect.

![rest](images/rest.png)

### 5. Open the inventory

Press **Y** to open the inventory screen. `inventoryText` and `inventoryBlack` confirm the inventory UI and trigger the **Inventory** effect.

![inventory](images/inventory.png)

### 6. Fight a boss

When a boss encounter begins, the boss name banner and its red health bar appear at the top of the screen. `bossBarRed` reads the red boss bar and `bossText` reads the boss name text; together they trigger the **Boss** effect.

![boss](images/boss.png)

### 7. Collect an item

Pick up a new item so the item card appears. The grey card panel (`itemGrey`) together with the item name text (`itemText`) triggers the **Item** effect.

![item](images/item.png)

---

## Notes

* **In-game detection gates most effects** — guard, damage, low health, coins, gloom, boss and item all require the in-game state. Death, rest and inventory are read on their own screens.
* The Health and Resolve bars are persistent HUDs whose meters (`healthBarRed`, `resolveBarYellow`) track live fill, so their effects react to *changes* rather than a fixed level. Use **Bar Effect Adjust** to position them.
* Meter validation alone does not guarantee the integration is working. A complete test should verify both:
  * Meter validation (correct region and color detection)
  * Effect validation (the lighting effect actually triggers and looks correct)

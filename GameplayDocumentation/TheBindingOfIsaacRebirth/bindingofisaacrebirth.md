# The Binding of Isaac: Rebirth

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

Change the monitor resolution, launch the game and open **Options → Video**.

Verify that the selected resolution appears in the game's display settings, then set the game to the resolution being tested. If it is not available, adjust the display/aspect-ratio setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

---

## Testing Guidelines

Observation: Most effects only fire while the integration considers you **"in game"** (an active run HUD is visible). The in-game state is detected from the HUD's black region plus the coin counter (`inGameBlack` + `coinWhite`), so start a run first, then trigger the individual states below. **Death** and **Level Up** are the exceptions — they fire on their screens while *not* in game.

1. Start a run. Once the run HUD is visible — the row of red hearts top-left and the coin / bomb / key counters below — the in-game state activates, triggering the `inGameBlack` meter (gated together with `coinWhite`).

![ingame](images/ingame.png)

2. Take a hit and lose part of a red heart. The drop in the red health row triggers the `healthBarRed` meter (Hurt effect).

![health](images/health.png)

3. Pick up soul (blue) hearts so the blue heart row is present. This drives the `healthBarBlue` meter (Eternal effect); a soul heart also buffers damage and cancels the low-health warning.

4. Get down to your last red heart (roughly half to one heart, no soul hearts). The near-empty red row triggers the `lastHealthRed` meter (Low Health effect).

5. Pick up a coin so the coin counter changes. The change in the white coin digits triggers the `coinWhite` meter (Coin Pickup effect).

![coin](images/coin.png)

6. Pick up a bomb so the bomb counter changes. This triggers the `BombWhite` meter (Bomb Pickup effect).

![bomb](images/bomb.png)

7. Pick up a key so the key counter changes. This triggers the `keyWhite` meter (Key Pickup effect).

![key](images/key.png)

8. Pick up an item. The item pickup banner / pedestal art triggers the `itemWhite` and `itemBlack` meters together (Item Pickup effect).

![item](images/item.png)

9. Enter a boss fight so the boss health bar appears at the top of the screen. The red bar triggers the `bossBarRed` and `bossBarNotRed` meters (Boss effect).

![boss](images/boss.png)

10. Clear the floor and take the trapdoor to the next level. The new-floor / stats transition triggers the `levelUpWhite` and `levelUpBlack` meters (Level Up effect — fires while *not* in game).

![level](images/level.png)

11. Let an enemy kill you. The **Game Over** screen triggers the `deathWhite1`, `deathWhite2` and `deathNotWhite` meters (Death effect — fires while *not* in game).

![death](images/death.png)

---

## Notes

* **In-game gating** — most effects require the run HUD to be visible (`inGameBlack` + `coinWhite`). If in-game detection is not calibrated, downstream effects will appear to never trigger.
* **Coin / Bomb / Key fire on change** — these effects trigger only when the on-screen counter *changes* during a run (a real pickup/spend), not from the static count. Leaving and re-entering a run with the same counts will not re-fire.
* Meter validation alone does not guarantee the integration is working. A complete test should verify both:
  * Meter validation (correct region and color detection)
  * Effect validation (the lighting effect actually triggers and looks correct)

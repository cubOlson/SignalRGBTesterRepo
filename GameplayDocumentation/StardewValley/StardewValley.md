# Stardew Valley

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

Change the monitor resolution, launch the game and open the options menu.

Verify that the selected resolution appears in the Display / Resolution option, then set the game to the resolution being tested. If it is not available, adjust the Window Mode setting so the resolution can be selected.

Test the game in fullscreen or windowed borderless.

**Keep the default HUD.** The meters read fixed spots in the corners of the screen: the clock and weather icon in the upper-right, and the Energy (**E**) and Health (**H**) bars in the lower-right. Do not change the **UI Scale** or hide the HUD.

---

## Effect Controls

Each effect has its own on/off toggle in the SignalRGB UI: **Low Energy Effect**, **Low Health Effect** and **Weather Effects**. The remaining controls are:

* **Ambience Brightness** — how strongly the screen is mirrored onto the devices. It dims automatically while a weather, low health or low energy effect is playing so the effect stands out.
* **Background Color** — the solid color behind the ambience.

Every effect only fires while the integration considers you **in game** (the HUD clock is visible, see step 1).

---

## A Note on Testing Stardew Valley

Stardew Valley is slow to test because most states depend on **the calendar** and **where you are**:

* **Weather** changes with the day and season, so you need to reach the right day in the right season, or load a save that is already on one. There is a quick reference table in step 8.
* **Low Health** only works where the Health bar is shown: **The Mines, the Secret Woods, the Slime Hutch, the Skull Cavern, the Mutant Bug Lair, the Quarry Mine and the Volcano Dungeon**. The bar is hidden everywhere else.
* **Low Energy** works anywhere, as the Energy bar is always visible.

**One screen can drive several meters**, so you do not need a separate capture per meter. A single frame of the lower-right bars, for example, validates both the energy and health meters that read them.

> **Before you start**, prepare saves or a day on each season you need. If possible, keep one save in **Summer**, because it holds most of the weather states (rain, thunder and green rain).

---

## Testing Guidelines

### 1. Enter the game — in game

Load a save and walk around the farm. As soon as the clock panel in the upper-right appears, the in-game state activates. `inGame1` and `inGame2` (two small points on the clock panel) are added together and must reach about **1.8** to count, so both have to match. **Every effect below requires this state.** In menus, cutscenes and the title screen, no effect fires.

![ingame](images/ingame.png)

### 2. Low energy

Work until your energy is almost gone (swing a tool, or run without eating). The Energy bar in the lower-right corner empties from the top: the upper segments `energyVal` and `energyVal2` must read empty (0), while the bottom segment `energyVal3` still reads above 0 and at most **0.5**, with `energyCheck` (the **E** label) confirming the bar is on screen. A red bar sweeps back and forth across the devices.

The effect stops when you recover energy (the bottom segment goes back above 0.5, for example after eating).

![low energy](images/low_energy.png)

### 3. Low health

Go to one of the places listed in the note above (the easiest is **The Mines**) and take damage until the Health bar is almost empty. The **H** bar sits next to the Energy bar. `healthCheck` (the **H** label) confirms the bar is visible, `healthVal` and `healthVal2` must read empty, and `healthVal3` must be above 0 and at most **0.5**. Red drops fall across the devices.

It stops when your health recovers, or when you leave the area.

![low health](images/low_health.png)

### 4. Sunny day

On a clear day, the weather icon in the clock panel shows the sun. `sunYellow` reads high and keeps the weather effect off. A sunny day never starts a weather animation, and it resets the weather state if another one was running.

![sun](images/sun.png)

### 5. Rain

On a rainy day (spring and summer), the umbrella icon appears in the clock panel. `umbrellaRain` plays falling blue raindrops. The ambience dims while it runs.

![rain](images/rain.png)

### 6. Thunderstorm

On a stormy day, the lightning icon appears. `lightningWhite` plays rain plus a lightning bolt every few seconds, and darkens the devices slightly.

![lightning](images/lightning.png)

### 7. Wind — petals and falling leaves

* **Petals (spring and summer wind days)** — the pink petals icon triggers `petalsPink`.
* **Falling leaves (fall)** — the leaf icon triggers `fallLeaves`.

Both play the same effect: pink petals drift slowly across the devices.

| Petals | Falling leaves |
| --- | --- |
| ![petals](images/petals.png) | ![fall leaves](images/fall_leaves.png) |

### 8. Snow, green rain and festival days

* **Snow (winter)** — the snowflake icon triggers `snow`. Grey snowflakes fall. It only fires when the petals meter is low, so the two icons are not confused.
* **Green Rain (summer)** — the green umbrella icon triggers `umbrellaGreenRain`. Green drops fall and the ambience dims.
* **Festival** — the festival icon triggers `festivalBlue`. Blue raindrops fall.

| Snow | Green rain | Festival |
| --- | --- | --- |
| ![snow](images/snow.png) | ![green rain](images/green_rain.png) | ![festival](images/festival.png) |

#### Weather reference

Dates used while testing (from the test notes for this integration):

| Weather | Season and day |
| --- | --- |
| Rain | Spring 3 and 7, Summer 3 |
| Thunderstorm | Summer 4, 13, 16, 20 and 26 |
| Green Rain | Summer 5, 6, 7, 14, 15, 16, 18 and 23 |
| Petals | Spring 21, Summer 8 |
| Falling leaves | Any day in Fall |
| Snow | Any day in Winter |
| Festival | Spring 13, Summer 11, Winter 8 |
| Sun | Any clear day |

> On a player's **wedding day** a heart replaces the weather icon, so no weather effect is shown.

---

## Notes

* **In-game detection gates every effect** — if nothing fires, check the HUD and the clock panel first.
* **One weather at a time** — while a weather effect is running, another one cannot start. A weather effect ends when its icon disappears (or when you leave the game), and a sunny icon resets the state.
* **Low Energy and Low Health need partially filled bars** — the effects read the bottom segment of each bar, not the whole bar, so a bar that jumps from full to empty may not trigger them. Drain it gradually.
* **Low Health only appears in dangerous areas** — the Health bar is hidden on the farm and in town.
* **Screenshot frames in this folder** — the QA screenshots in `SDR/` are organized per meter, and the suffix of each file name is the match threshold used by the meter (for example `inGame1_gte.95` = at least 0.95, `energyVal3_lte.5` = at most 0.5). Only 3840x2160 SDR frames are included, so other resolutions still need their own captures.
* Meter validation alone does not guarantee the integration is working. A complete test should verify **both**:
  * **Meter validation** — the region and color detection are correct (use the QA tool with a screenshot at the target resolution).
  * **Effect validation** — the lighting effect actually triggers and looks correct.

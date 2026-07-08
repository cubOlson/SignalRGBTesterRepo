# SOP — Game Integration Tester (SignalRGB Test Harness)

**Purpose:** Standardize how a tester validates a generated game-integration HTML file in the SignalRGB test harness so that both **meters** and **lighting effects** are confirmed before an integration ships.

**Applies to:** Anyone validating a `<Game>.html` integration produced by the Game Integration Bot (see [game-integration-bot-sop.md](game-integration-bot-sop.md)).

**Owner:** Game Integrations team, SignalRGB.

---

## 1. Scope & Definitions

- **Test harness** — SignalRGB's built-in developer/test mode used to load an integration HTML file and run it against a live game or captured screenshots.
- **QA Tool** — the harness feature that loads a screenshot from a target resolution and shows exactly which pixels a meter region samples and at what color. Primary tool for diagnosing meters.
- **Meter validation** — confirming each vision meter triggers at the right place, at the right time.
- **Effect validation** — confirming the lighting effect that the meter drives actually renders and looks correct.

> A complete test = **meter validation + effect validation**. A meter that triggers but produces no/incorrect lighting is still a failure to investigate.

---

## 2. Preconditions

1. **Sync from remote:** `git pull` before you begin (repo rule — always update before pushing; see [README.md](../README.md)).
2. Confirm you have the integration file at the repo root and its guide under [GameplayDocumentation/](../GameplayDocumentation/) `<game>/<game>.md`.
3. Confirm the game is installed and can reach the states listed in the guide (test world, practice lobby, etc.).
4. Read the integration's `description` meta and note the **version** you are testing — you will record it in the sign-off.

---

## 3. Environment Setup

1. Open the SignalRGB test harness and load the target `<Game>.html`.
2. Set the game's video settings to a **supported resolution** and confirm the in-game resolution matches the monitor resolution (see each guide's *Resolution Setup*).
3. Test the **full supported-resolution matrix** across the run, not just one. Standard set:
   `1920x1080, 1920x1200, 2560x1080, 2560x1440, 2560x1600, 3440x1440, 3840x1080, 3840x2160, 5120x1440`, plus **`3840x2160 (HDR)`**.
   - The HDR variant uses the `hdr-h / hdr-s / hdr-l` meter ranges — test it explicitly.
4. Verify the harness loads the file with no console errors and the `320×200` preview canvas renders.

---

## 4. Test Procedure

Follow the game's `GameplayDocumentation/<game>/<game>.md` guide step by step. For each documented game state:

1. **Reproduce the state** in-game exactly as the guide describes (enter menu, score, take the action, etc.).
2. **Meter check** — confirm the expected meter(s) trigger. Recall the positive/negative pairing: the state is only valid when the **positive** meter reads high **and** the paired **"Not"** meter reads low. Watch for false triggers during transitions.
3. **Effect check** — confirm the corresponding lighting effect renders on the preview/devices, is the right effect, and looks visually correct (color, motion, timing).
4. **Record** pass/fail for both the meter and the effect.

Repeat the full sequence for each resolution in the matrix, paying attention to ultrawide and HDR where meter regions most often drift.

---

## 5. Diagnosing Failures with the QA Tool

If an effect never fires:

1. Open the **QA Tool** and load a screenshot from the target resolution.
2. Verify the meter region actually captures the right pixels at the right color.
3. If the region is off or the HSL range is wrong, note the correct coordinates/range.

Classify each failure:
- **Meter triggers, no/incorrect effect** → effect logic issue. Still a failure; flag for investigation.
- **Meter never triggers** → region/HSL/resolution issue. Capture the QA Tool evidence.
- **False trigger** → likely a missing or mis-tuned negative "Not" meter.

Do **not** hand-patch coordinates as the fix of record — report findings so the file is regenerated/corrected and re-versioned by the bot operator.

---

## 6. Results Log (record per integration)

For each test run, record:

- Game and integration **version** tested.
- Resolutions tested (and HDR yes/no).
- Per-state result: meter pass/fail, effect pass/fail.
- QA Tool findings for any failure (screenshot + region/HSL notes).
- Overall verdict: **Pass** / **Pass with notes** / **Fail**.

---

## 7. Sign-off

On a full pass (all documented states validated for both meters and effects across the resolution matrix):

1. Add or update the tested-by comment in the integration's `<head>`, matching the repo convention:
   ```html
   <!-- Tested by <Name> at <M/D/YYYY> -->
   ```
2. Commit the tested file (and any updated documentation/screenshots).
3. `git pull` to catch remote changes, then push.
4. Notify the team that the integration (with its version) has passed.

If the result is **Fail** or **Pass with notes**, return the results log to the bot operator for a fix and version bump — do not add a tested-by sign-off.

---

## 8. Escalation

- **Meters correct but effects broken** → escalate to the integration author; note meter-vs-effect split clearly.
- **Region drifts only at specific resolutions** → capture QA Tool evidence per resolution so the fix can target the right `<resolution>` variants.
- **Game UI changed since generation** → integration must be regenerated and re-versioned, not patched during testing.

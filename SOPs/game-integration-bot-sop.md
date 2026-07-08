# SOP — Game Integration Bot

**Purpose:** Standardize how the Game Integration Bot generates SignalRGB game-integration HTML effect files so that every generated integration is structurally consistent, correctly versioned, and ready for the tester (see [game-integration-tester-sop.md](game-integration-tester-sop.md)).

**Applies to:** Anyone operating the bot to produce or regenerate a `<Game>.html` integration in the [repo root](../).

**Owner:** Game Integrations team, SignalRGB.

---

## 1. Scope & Definitions

- **Integration file** — a single self-contained `<Game>.html` at the repo root (e.g. [MecchaChameleon.html](../MecchaChameleon.html)). It contains the metadata, user-facing controls, vision meters, and the JavaScript render loop for one game.
- **Vision meter** — a `<meta meter=...>` region that samples a rectangle of the game screen and reports what percentage of pixels fall inside an HSL range. Meters drive the lighting effects.
- **The bot** — the automation/AI tool that generates the integration file from a game's UI/gameplay signals.

The bot **generates**; it does not sign off. Every generated file must go through the tester before it ships.

---

## 2. Preconditions (before running the bot)

1. Confirm the game is on the current **top-ten testing list** for the month (see [README.md](../README.md)). If a game dropped out of the top ten, do **not** delete its existing file.
2. **Sync from remote before starting:** `git pull` (see repo rule in [README.md](../README.md) — always update from remote before pushing new code). Work on a branch if you want.
3. Gather source material for the game:
   - Reference screenshots at each supported resolution.
   - The list of game states that should trigger lighting (round start, score, menus, etc.).
4. Confirm the target supported resolutions. The standard set used across integrations is:
   `1920x1080, 1920x1200, 2560x1080, 2560x1440, 2560x1600, 3440x1440, 3840x1080, 3840x2160, 5120x1440,` and a `3840x2160 (HDR)` variant.

---

## 3. Required Output Structure

Every file the bot emits **must** contain the following, in this order.

### 3.1 Head metadata
```html
<head>
  <title>Game Name</title>
  <meta description="Short effect description. vX.Y.Z" />
  <meta publisher="SignalRGB" />
```
- `title` — human-readable game name.
- `description` — one short sentence **plus a semantic version** `vMAJOR.MINOR.PATCH`. The version is mandatory; bump it on every regeneration (see §6).
- `publisher` — `SignalRGB` unless a legacy publisher must be preserved.

### 3.2 User-facing controls
Active `<meta property=...>` controls appear in the SignalRGB UI. Each control **must** have a `label` and a plain-language `tooltip`. Group them under a clear comment banner (`ACTIVE USER-FACING CONTROLS`). Keep unused template properties commented out for reference rather than deleting them.

Standard baseline controls the bot should always include:
- `ambienceAlpha` — background brightness (0–100).
- `backgroundMode` — background style list (Color Cycle / Ambience / Audio Visualizer / Custom, etc.).
- `backColor` — custom solid color.
- One `boolean` toggle per discrete effect so users can disable individual effects.

### 3.3 Vision meters
For each detectable game state, emit a `<meta meter=...>` block with:
- Base `x / y / width / height` (normalized 0–1) and HSL ranges `h / s / l`, plus `hdr-h / hdr-s / hdr-l` for the HDR path.
- Nested `<resolution>` entries covering both **aspect ratios** (e.g. `aspect="3.55:1"`) and **specific sizes** (e.g. `size="3840x2160"`) for every supported resolution.
- A `Last edited:` timestamp comment above the block.

**Detection strategy (mandatory):** every game state must use a **positive** meter (confirms the expected color *is* present) paired with a **negative "Not" meter** (confirms the region does *not* show that color in the baseline/off state). A state is active only when positive is high **and** negative is low. This prevents false positives from ambient lighting and UI transitions.

Document each meter group with a comment explaining what it samples and which callback it fires.

### 3.4 Render script
- A `320×200` canvas (`<canvas id="exCanvas" width="320" height="200">`) — SignalRGB scales this onto the device layout.
- A hidden off-screen canvas that all effects draw into each frame, then composite onto the main canvas.
- One `new Meter(size, callback)` per meter, smoothing the vision signal over N frames.
- An `update()` loop that: clears to black → lays the background → pushes `engine.vision.<meterName>` into each meter via `setValue()` → composites.
- User-facing property values are injected into JS scope by name; reference them (not hard-coded literals) so the UI controls actually work.

---

## 4. Generation Procedure

1. Run the bot with the game's source material and the supported-resolution set.
2. Verify the emitted file against the **Output Checklist** (§5).
3. Save as `<Game>.html` at the repo root using the existing naming convention (PascalCase, no spaces — match neighbors like `Battlefield6.html`, `ForzaHorizon5.html`).
4. Create/refresh the matching test guide under [GameplayDocumentation/](../GameplayDocumentation/) `<game>/<game>.md` listing supported resolutions, resolution setup, and step-by-step trigger instructions with screenshots. The tester relies on this.
5. Do **not** add a `Tested by` comment — that is added by the tester after validation, not the bot.

---

## 5. Output Checklist (bot operator must confirm before handoff)

- [ ] `title`, `description` **with `vX.Y.Z`**, and `publisher` present.
- [ ] Every active property has both `label` and `tooltip`.
- [ ] Every effect has a user toggle.
- [ ] Every game state has a positive **and** a negative "Not" meter.
- [ ] Every meter includes `<resolution>` variants for all supported aspect ratios and sizes, including the HDR path.
- [ ] Canvas is `320×200`; `update()` clears→background→setValue→composite.
- [ ] No hard-coded values where a user property should be referenced.
- [ ] Matching `GameplayDocumentation/<game>/` guide created or updated.
- [ ] File name matches repo convention; file is valid HTML and opens without console errors.

---

## 6. Versioning

- New integration → `v1.0.0`.
- Bug fix / meter tweak, no behavior change → bump **patch** (`v1.0.1`).
- New effect or control → bump **minor** (`v1.1.0`).
- Breaking change to structure or removed effects → bump **major** (`v2.0.0`).
- The version in the `description` meta is the source of truth.

---

## 7. Handoff & Commit

1. Confirm the Output Checklist is complete.
2. `git pull` again to catch any remote changes, then commit the `<Game>.html` plus its `GameplayDocumentation/` guide together.
3. Use a descriptive commit message (e.g. `Add MecchaChameleon integration` or `Bump Minecraft to v1.1.1`).
4. Push, then hand off to the tester by pointing them at the file and its documentation.

---

## 8. Escalation

- **Meter never fires / wrong region** → this is expected to be caught and tuned by the tester using the QA Tool. Note any known weak spots in the handoff.
- **No reliable positive/negative color pair** for a state → flag to the team; do not ship a single-meter detection.
- **Game UI changed** → regenerate and bump the version rather than hand-editing coordinates ad hoc.

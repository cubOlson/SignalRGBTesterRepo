# VALORANT

## Supported Resolutions

- 1920x1080
- 1920x1200
- 2560x1080
- 2560x1440
- 2560x1600
- 3440x1440
- 3840x1080
- 3840x2160
- 5120x1440
- 3840x2160 (HDR)

---

## Resolution Setup

This is the most important step.

Always go to **Settings → Video Settings** to verify and adjust the in-game resolution.

VALORANT does not always correctly adapt resolution changes made in the operating system, so manual adjustment in-game is required.

### Resolution Change Procedure

1. Select the desired resolution in the game settings
2. Set **Display Mode** to:
   - Windowed Fullscreen
3. Click **Apply**

If the resolution does not apply correctly:

1. Switch **Display Mode** to Fullscreen
2. Select the desired resolution
3. Switch back to Windowed Fullscreen
4. Click **Apply**

---

## Supported Modes

- Custom Game
- Swiftplay
- Spike Rush

---

## Custom Match Setup (IMPORTANT)

Before starting the match:

1. Go to **Play → Custom Game**
2. Open game options and enable:
   - Allow Cheats: ON


---

## Core Testing System (Custom Match Loop)

Each test cycle follows this structure:

1. Start Custom Match
2. Select agent
3. Enable Infinite Abilities (ON → OFF)
4. Trigger ability meter (X ability)
5. End Game Phase
6. Select next agent
7. Repeat

---

## First Custom Match

### Initial Agent: Jett

1. Join **Defenders** team 
2. Select **Jett**
3. Start the match

4. Open cheats menu:
   - Press **ESC → Cheats**
   - Enable **Infinite Abilities ON → OFF**

5. Trigger:
   - Jett X ability meter
   - In-game ability meters

![Jett Test](images/jett.png)

---

## Agent Rotation Cycle (Custom Matches)

After each round:

1. Open **ESC menu**
2. Select "Agent for Next Round"
3. Select an agent from the list
4. Click **End Game Phase**

---

### Chamber

- Enable Infinite Abilities ON/OFF
- Use X ability (Weapon Sight)
- Trigger Chamber meter

![Chamber Test](images/chamber.png)

---

### Neon

- Enable Infinite Abilities ON/OFF
- Trigger Neon X abilities
- Capture Neon meters

![Neon Test](images/neon.png)

---

### Viper

- Enable Infinite Abilities ON/OFF
- Trigger X abilities
- Capture Viper meter

![Viper Test](images/viper.png)

---

### Phoenix

- Enable Infinite Abilities ON/OFF
- Use abilities to replicate:
  - Reyna E1 meter behavior
- Trigger Phoenix X ability meter

![Phoenix Test](images/phoenix.png)

---

## Match Progression Rule

- After agent rotation do **End Game Phase** until **Match 12**

---

## Match 12 Transition (Side Swap)

After Match 12:

1. Switch team:
   - Defenders → Attackers

2. Final test:
   - Pick Spike
   - Plant Spike
   - Bomb Plated

This triggers:
- Spike planted meters
- Bomb Plated meters
- Victory meters
![Spike Test](images/spike.png)
![Bomb Test](images/bomb.png)
![Victory Test](images/victory.png)

---

## Second Custom Match

1. Start a new Custom Match
2. Join the **Attackers** team
3. Repeat End Game Phase until Match 12

---

### Final Match Swap (Defeat Setup)

At Match 12:

1. You are now on the **Defenders**
2. Switch to **Attackers**
3. Plant Spike
4. Switch back to **Defenders**
5. Wait for Spike explosion

This triggers:
- Defeat meters
![Defeat Test](images/defeat.png)

---

## Swiftplay / Spike Rush (Additional Meter Collection)

Use these modes for remaining meters:

### Used for:
- Reyna E2
- Shield meters
- Dead meters
---

### Shield Meters

1. Enter Swiftplay or Spike Rush
2. Buy Shield in shop

![Shield Test](images/shield.png)

---

### Reyna Meter

- Play normally
- Use Reyna ability interactions
- Trigger soul orb / ability effect meters

![Reyna Test](images/reyna.png)

---

### Dead Meters

- Get killed
- Trigger dead meters

![Dead Test](images/dead.png)

---

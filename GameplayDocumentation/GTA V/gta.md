# Grand Theft Auto V

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

Open the video settings.

Verify the following setting:

- **Screen Type:** Windowed Borderless

Also verify that the selected resolution is correct.

This allows the integration to properly adapt to all supported resolutions.

---

## Testing Guidelines

1. Launch the game and enter:
   - **Story Mode**

2. After loading into the game, the following meter will trigger automatically:
   - **Blue Bar**
![Ball Out Meter](images/bluebar-meter.png)

3. Travel to a populated area with multiple NPCs.

4. Attack civilians in order to trigger:
   - **Money Black**
   - **Money Green**
![Money Meter](images/money-meter.png)

5. Continue attacking civilians until the police become alerted.

6. Trigger the following wanted level meters:
   - **First Star**
   - **First Star Black 1**
   - **First Star Black 2**
![Wanted Meter](images/wanted-meter.png)

7. As you are wanted, continue to attack people to get the money 2 meters.
    - **Money Green 2**
    - **Money Black 2**
![Money2 Meter](images/money2-meter.png)

8. Allow the police to shoot the player repeatedly.

9. Attack the police while continuing to take damage.

   This triggers:
   - **Low HP**
![Low Health Meter](images/lowhealth-meter.png)

10. Allow the police to kill the player.

    This triggers:
   - **Wasted**
![Wasted Meter](images/wasted-meter.png)

11. After respawning from the hospital or police station, the following meters will trigger:
   - **Money Red**
![Money Respawn Meter](images/moneyrespawn-meter.png)

---

## GTA Online Testing

1. Enter:
   - **GTA Online**

2. Repeat the same process used in Story Mode:
   - Attack civilians
   - Trigger wanted levels
   - Engage police

3. Allow the police to kill the player.

   This triggers:
   - **Wasted White**
![Wasted White Meter](images/wastedwhite-meter.png)

---


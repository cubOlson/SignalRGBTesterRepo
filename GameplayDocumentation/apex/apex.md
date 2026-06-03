# Apex Legends

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

- **Display Mode:** Fullscreen Windowed

Also verify that the selected resolution is correct.

This allows the integration to properly adapt to all supported resolutions.

---

## Testing Guidelines

1. Launch the game and enter:
   - **Bot Royale**

2. During Legend Selection, the following meters will trigger:
   - **Select Menu**
   - **Select Menu Color**

   Select **Valkyrie** as the first legend.

![Legend Select](images/legend-select.png)

3. During the Dropship phase, the following meters will trigger:
   - **Dropship Orange**
   - **Dropship White**

![Dropship](images/dropshipwhite.png)
![Dropship2](images/dropshiporange.png)

4. After landing, the following meters will trigger automatically:
   - **White In-Game**
   - **Orange In-Game**
   - **Check**
   - **Gray**
   - **Health**
   - **Teammate 1**
   - **Teammate 2**

![In Game HUD](images/in-game-hud.png)

5. Press **ESC** while in-game to trigger:
   - **osdEsc**

![Pause Menu](images/pause-menu.png)

6. Wait until Valkyrie's Ultimate becomes available.

   This will trigger:
   - **ULT Check**
   - **ULT Call**
   - **White Valk**

![Valkyrie Ultimate Ready](images/valk-ult-ready.png)

7. Activate Valkyrie's Ultimate.

   This will trigger:
   - **Valk Z**

![Valkyrie Ultimate](images/valk-ultimate.png)

8. During gameplay, acquire shields and trigger:
   - **Shield Bar**
   - **Shield Bar Color**

![Shield Meter](images/shield-meter.png)

9. As the match progresses and the ring begins closing, trigger:
   - **Orange Storm Bar**
   - **White Storm Bar**

![Storm Meter](images/storm-meter.png)

10. Take shield damage and use a Shield Cell or Shield Battery to trigger:
    - **Shield Charge**

![Shield Charge](images/shield-charge.png)

11. Take health damage and heal using a Syringe or Med Kit to trigger:
    - **Syringe Charge**

![Health Charge](images/health-charge.png)

12. Allow yourself to be knocked down while a teammate is nearby.

    This will trigger:
   - **Revive**

13. Have a teammate revive you.

    This will trigger:
   - **Revive Confirm**

![Revive](images/revive.png)

14. While knocked down and waiting to be revived, trigger:
   - **osdEsc On**

![Knocked Down](images/knocked-down.png)

15. Reduce your health to a critical level to trigger:
   - **Low Health**

![Low Health](images/low-health.png)

16. Win a match to trigger:
   - **Win Gold**
   - **Win Loss Confirm**

![Victory](images/victory.png)

---

## Rampart Testing

1. Start a new Bot Royale match.

2. Select:
   - **Rampart**

3. During gameplay, trigger Rampart's tactical ability.

   This will trigger:
   - **Q**
   - **Q1**
   - **Q2**
   - **Q3**

![Rampart Tactical](images/rampart-q.png)

4. Wait until Rampart's Ultimate becomes available.

5. Activate Rampart's Ultimate.

   This will trigger:
   - **Mini**

![Rampart Ultimate](images/rampart-ultimate.png)

6. Allow your entire squad to be eliminated.

   This will trigger:
   - **Lost Red**Kv
   - **Win Loss Confirm**

![Squad Eliminated](images/squad-eliminated.png)

---

## Crypto Testing

1. Start a new Bot Royale match.

2. Select:
   - **Crypto**

3. Wait until Crypto's Ultimate becomes available.

4. Deploy the drone and activate the Ultimate.

   This will trigger:
   - **CYRP**

![Crypto Ultimate](images/crypto-ultimate.png)

---

## Phoenix Kit Testing

1. Continue playing matches and looting containers until you find a:
   - **Phoenix Kit**

2. Take damage to both health and shields.

3. Use the Phoenix Kit.

   This will trigger:
   - **Phoenix Kit**

![Phoenix Kit](images/phoenix-kit.png)

---

## Loba Testing

1. Start a new Bot Royale match.

2. Select:
   - **Loba**

3. Wait until Loba's Ultimate becomes available.

4. Activate Loba's Ultimate.

   This will trigger:
   - **Loba**

![Loba Ultimate](images/loba-ultimate.png)
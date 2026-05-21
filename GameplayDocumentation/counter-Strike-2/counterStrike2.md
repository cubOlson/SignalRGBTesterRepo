# Counter-Strike 2

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

After changing the resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the **Resolution** option. If the resolution is not available:

- Change the **Aspect Ratio** setting.
- Check which aspect ratio matches the resolution being tested.
- Select the correct resolution again after changing the aspect ratio.

The game only supports exclusive **Fullscreen** mode when using the monitor’s native resolution. For all other resolutions, use **Fullscreen Windowed** mode.

---

## Testing Guidelines

1. Launch the game and select **Practice Mode**, then choose **Casual**.

2. Choose any map and join the **Terrorist** team.

3. At the start of the round, several in-game and Terrorist-related meters will trigger automatically.
![Terrorist HUD](images/terrorist-hud.png)

4. Open the shop menu clicking 'B' to trigger the **Shop** meters.
![Shop Menu](images/shop-menu.png)

5. Move to a bomb site and plant the bomb to trigger the bomb-related meters.
![Bomb Planted](images/bomb-planted.png)

6. During the round, trigger combat-related meters by:
   - Killing enemy players to activate **Kill** meters.
   ![Kill Example](images/kill.png)
   - Taking damage to activate **Low Health** meters.
    ![Low Health Example](images/low-health.png)

7. Win at least one round as the **Terrorist** team and one round as the **Counter-Terrorist** team.
![Terrorist Round Win](images/t-round-win.png)
![Counter-Terrorist Round Win](images/ct-round-win.png)

8. Open the settings menu, switch teams, and join the **Counter-Terrorist** team.

9. At the start of the round, several in-game and Counter-Terrorist-related meters will trigger automatically.
![Counter-Terrorist HUD](images/counter-terrorist-hud.png)

10. Allow the enemy team to kill you.
    
11. After dying, enter **Spectator** mode.
    - Use the right/left mouse button to switch between players.
    - Cycle through both Terrorist and Counter-Terrorist players to trigger all Spectator meters for both teams.
    - Kill at least one enemy player before entering Spectator mode to trigger the queue-related meter.
    ![Spectator Mode](images/spectator-mode.png)

12. Lose one round to trigger the **Round Loss** meters.
![Round Loss](images/round-loss.png)

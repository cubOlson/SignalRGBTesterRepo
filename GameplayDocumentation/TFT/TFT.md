**Resolutions**

- [ ] 1920x1080  
- [ ] 1920x1200  
- [ ] 2560x1080  
- [ ] 2560x1440  
- [ ] 3440x1440  
- [ ] 3840x1080  
- [ ] 3840x2160  
- [ ] 5120x1440

**Resolution Setup**

Change the monitor resolution, launch the game and open the video settings.

Verify that the selected resolution appears in the Resolution option, then change the game resolution to the right one. If the resolution is not available:

\- Change the Aspect Ratio setting.

\- Check which aspect ratio matches the resolution being tested.

Test the game in fullscreen or windowed borderless.

**Testing Guidelines**

1\. Launch the game, select a normal match and wait for the match to start to trigger the inGame meter.  
![inGame](images/inGame.png)

2\. After a few seconds, once the HUD with the shop loads, check the defaultUI and TimeBar meters.  
![defaultUI](images/defaultUI.png)  
![timeBar](images/timeBar.png)

3\. When the TimeBar reaches zero and you enter combat, check the CombatConfirmations and StateSwitch meters.  
![combatConfirmation](images/combatConfirmation.png)  
![stateSwitch](images/stateSwitch.png)

4\. When combat ends and you move to the planning phase, check the planningConfirmation meter.  
![planningConfirmation](images/planningConfirmation.png)

5\. Upon winning a combat, an orange flame icon appears next to the gold count, triggering the Win meter.  
![win](images/win.png)

6\. Upon losing a combat, the flame icon next to the gold count turns blue, triggering the Lose meter.  
![lose](images/lose.png)

7\. As you accumulate gold, check the moneyCount meter.  
![moneyCount](images/moneyCount.png)

8\. Upon leveling up and gaining a slot to place a champion, check the lvlUp meter.  
![lvlUp](images/lvlUp.png)

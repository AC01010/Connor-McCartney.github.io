---
title: How I Customise EndeavourOS KDE Plasma
categories:
- Other
excerpt: |
  
---

#### 1. Update mirrors
<br>

#### 2. Remove auto startup page and notifications
<br>
```
sudo pacman -Rsn eos-update-notifier
```

#### 3. Update system
<br>
```
sudo pacman -Syu
```

#### 4. Configure shortcuts (these are just my preferences)

Open shortcuts

Delete the following:<br>
Quick tile window to the top <br>
Quick tile window to the bottom <br>

Change the following:<br>
If control+alt+t doesn't open terminal, click add application, search konsole and add shortcut. <br>
Show desktop grid: change to meta+tab <br>
Switch to next desktop: change to control+shift+right <br>
Switch to previous desktop: change to control+shift+left <br>

#### 5. Setup virtual desktops

Open virtual desktops <br>
I like to have 4. Turn navigation wraps around off. 


#### 6. Configure konsole

Open konsole<br>
Settings > Manage Profiles <br>
Create a new profile > Appearance > Choose a colour scheme <br>
Set this profile as default and apply<br>

Right click near top of konsole <br>
More actions > Configure special application settings <br>
Change exact match to substring match <br>
Add property > no titlebar and frame > yes > force <br>
Add property > maximised horizontally > yes > force <br>
Add property > maximised vertically > yes > force <br>

Settings > Toolbars shown > disable all
Settings > Show Menubar > disable

#### 7. Install paru
<br>
```
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```
To search with paru: paru -Ss  <br>
To install with paru: paru -S  <br>
To remove with paru: paru -Rsn 

#### 8. Remove firefox
<br>
```
paru -Rsn firefox 
rm -r ~/.mozilla 
rm -r ~/.cache/mozilla 
```

#### 9. Install whatever you want
<br>
```
paru -S brave-bin google-chrome discord vim btop grub-customizer
```
  
#### 10. Remove krunner desktop 

Add the following to ~/.config/kdeglobals <br>
[KDE Action Restrictions][$i] <br>
run_command=false

#### 11. Disable screen locking/power saving

Advanced power settings > Stop charging only once below > 80% <br>
Energy saving > On AC Power > turn everything off except dim screen and energy saving <br>
Screen locking > disable

#### 12. Grub Customizer

Disable look for other operating systems. <br>
Boot default entry after 1 second. <br>
Kernel parameters: quiet loglevel=1 nowatchdog nvme_load=YES fsck.mode=skip modprobe.blacklist=iTCO_wdt

#### 13. Mirage global theme, nebula desktop, nebula splash
<br>

#### 14. Desktop effects

Turn on wobbly windows, magic lamp, translucency, fall apart

#### 15. Fix monitor bug

Autostart > Add login script > killall plasmashell; sleep 3; plasmashell

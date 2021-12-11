---
title: How I Customise EndeavourOS Plasma
categories:
- Other
excerpt: |
  
---

This will be a series of steps I perform after a fresh install of EndeavourOS with the KDE Plasma desktop environment. 

#### 1. Update mirrors with welcome launcher
<br>
#### 2. Select don't show me anymore on welcome launcher
<br>
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
```
git clone https://aur.archlinux.org/packages/paru
cd paru
makepkg -si
```
To search with paru: paru -Ss <what you want to search> <br>
To install with paru: paru -S <what you want to install>
  

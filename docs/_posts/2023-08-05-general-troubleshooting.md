---
layout: post
title: "General troubleshooting"
date: 2023-08-02 11:10:02 +0545
categories: general
---

Find quick troubleshooting tips and useful commands here!

## Windows

To bring back old context menu on Windows 11,  run cmd as an administrator and enter the following command:

```
$ reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
```

## Linux

1. GNOME

Allow resizing with right mouse click
```
$ gsettings set org.gnome.desktop.wm.preferences resize-with-right-button true
```

Use Alt instead of Super:
```
gsettings set org.gnome.desktop.wm.preferences mouse-button-modifier '<Alt>'
```

### Run GUI apps as root in Sway
If you can't run GUI apps as sudo in Sway (error: no protocol specified or something similar),  run this in a terminal for a quick fix.

```
$ xhost + local:
```

### Swap Caps Lock and Escape keys
Find advanced options for keyboard in Gnome tweaks for Gnome and Keyboard settings for KDE.
For others, see below:

```
# Set keyboard options
#
# Put this in .xinitrc for startx
# .xsessionrc for Debian
setxkbmap -option caps:swapescape
```

### JAVA apps rendering issue in Wayland 
For JAVA apps having trouble rendering display on dwm or Sway, set the following environment variable:

```
# Put this in .xinitrc, .xsessionrc, .bash_profile
# or in a similar file
$ export _JAVA_AWT_WM_NONREPARENTING=1

# If the above doesn't work, try setting wmname to LG3D.
# This requires wmname to be installed.
$ wmname LG3D
```

## Others

### Node JS

#### Fix "error:0308010C:digital envelope routines::unsupported"

```
# Do this in the terminal where you use node commands
# Or set this in your package.json
$ export NODE_OPTIONS=--openssl-legacy-provider
```

#### Fix the upstream dependency conflict while installing NPM packages
```
$ npm install --legacy-peer-deps
```

### VS Code

#### Send Ctrl + P to terminal when terminal is open
Append this to your settings:

```
"terminal.integrated.commandsToSkipShell": ["-workbench.action.quickOpen"],
```

#### Use system keyboard layout
This is useful when you use advanced options with keyboard layout, eg, switching Caps Lock and Escape keys.

```
"keyboard.dispatch": "keyCode",
```

## Useful commands

1. Sort pacman packages by install date:
   ```
   $ pacman -Qi | grep -P "^(Name|Install Date)" | sed -r ":a;N;s/Name\s+: (.*)\nInstall Date\s+: (.*)/\2\t\1/" | while read pkg_date pkg_name; do echo -e $(date +%F\ %T --date="$pkg_date") '\t' $pkg_name; done | sort
   ```

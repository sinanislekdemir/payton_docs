---
title: "Installing Payton"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Installation is quite easy:


## Requirements:

- LibSDL2 `sudo apt install libsdl2-dev` for debian/ubuntu based linux distros. For other platforms, you can see your favourite package manager.
- imagemagick `sudo apt install imagemagick` for debian/ubuntu based linux distros. For other platforms, you can see your favourite package manager.
- Python 3.7+
- A Graphics card that supports OpenGL 3.3+

## Install:

From a bash terminal:
```bash
pip3 install payton
```

This should install all dependencies. If you get any permission errors, you are probably installing the library to system-global so missing some permissions. If you do not want to use pipenv or virtualenv, then you might want to run above command as `sudo pip3 install payton`

## Upgrade to the latest version:

Payton is under active maintenance. This means I am spending some time to fix the bugs or make it better. So you might want to upgrade it occasionally.

    pip3 install payton --upgrade
    
should do the trick!

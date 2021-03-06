---
layout: post
title:  "Raspberry Pi 3 setting - equipment"
date:   2017-09-28 20:26:38 +0900
tags: RaspberryPi Project Setting
---
# Precondition of developing Raspberry Pi 3
1. [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
2. MicroSD card and MicroSD card reader for mounting Raspbian OS
3. 2.5A Adapter
4. Monitor for output (requirements: HTMI), Keyboard and mouse for input (requirements: USB)
5. (Optional) Others: sensors (such as Motion Sensors, Environmental Sensors, Distance/Range Sensors and so on)

# Installing Raspbian on Raspbarry Pi 3
1. Download [Raspbian iso](https://www.raspberrypi.org/downloads/raspbian/) (recommend Raspbian Stretch with desktop not lite version)
2. Write an image, specifically Raspbian, on MicroSD card ([HOW TO](https://www.raspberrypi.org/documentation/installation/installing-images/README.md))
3. [More information](https://www.raspberrypi.org/help/)

# Raspbian Keyboard Configuration
1. Open console (shortcut: ctrl + alt + t)
2. Input command, 
```
pi@raspberrypi:~ $ sudo raspi-config
```
3. Than, you can see "Raspberry Pi Software Configuration Tool" window
4. Follow the options, Localisation Options > Change Keyboard Layout
5. Change keyboard configuration what you want to set

# Language Problem Solving
Broken font is the problem for using Korean.

1. Inatll ibus, ibus-hangle, fonts-unfonts-core
```
$ sudo apt-get install ibus -y
$ sudo apt-get install ibus-hangle -y
$ sudo apt-get install fonts-unfonts-core -y
```
2. Change Localisation of Raspberry Pi Configuration (Set Locale..., and Set Timezone...) to Korean
3. Rebute Raspbian
4. Change the ibus configuration on the right top, "EN" button, to Taegeuk mark

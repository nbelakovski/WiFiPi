WiFiPi
======

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution for easy setup, requiring no keyboard, monitor, mouse, and no modifications to the image.

Just throw this image, with NO modifications, onto an SD card, and pop it into your Pi. Your Pi is now broadcasting a WiFi network called WiFiPi,
password 'luggage12345'. Connect to it, at either `ssh pi@wifipi.local` or `ssh pi@10.0.0.5`. Default password is 'raspberry'.
Now you're in! Edit /boot/wifipi-wpa-supplicant.txt with your network information, or just go write some code!

This repository contains the source script to generate the distribution out of an existing `Raspbian <http://www.raspbian.org/>`_ distro image.

Where to get it?
----------------

Releases tab

How to use it?
--------------

#. Unzip the image and install it to an sd card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Boot the Pi from the card. No external peripherals are necessary.
#. Connect to WiFiPi, password is luggage12345
#. Log into your Pi via SSH (it is located at ``wifipi.local`` `if your computer supports bonjour <https://learn.adafruit.com/bonjour-zeroconf-networking-for-windows-and-linux/overview>`_ or at 10.0.0.5), default username is "pi", default password is "raspberry".
#. Edit /boot/wifipi-wpa-supplicant.txt to add the information of the wifi network you'd like to connect to. Optionally, edit /boot/wifipi-network.txt if you'd like to specify additional configuration parameters, like static IP address and so forth, BUT: be sure to edit /boot/wifipi-network.txt first, since AutoHotspot runs once per minute and uses whatever it finds in /boot/wifipi-wpa-supplicant.txt. If it runs while you're editing a file, your connection will get dropped.
#. AutoHotspot will run once per minute to check that the connection is still active, and launch a hotspot if it is not. You can disable it by running ``sudo crontab -e`` and putting a `#` in front of the line with all the asterisk (and in front of the reboot line if you like). You can see logs from AutoHotspot in /var/log/autohotspot.log, but this will get cleared daily.

Features
--------

* `AutoHotspot <http://www.raspberryconnect.com/network/item/331-raspberry-pi-auto-wifi-hotspot-switch-direct-connection>`_ Automatically launch a Direct Connection WiFi hotspot when no other connection is available.
* `Raspbian <http://www.raspbian.org/>`_ tweaked for maximum preformance in a headless configuration

Developing
----------

For development instructions, see the corresponding section at https://github.com/guysoft/CustomPiOS

Code contribution would be appreciated!

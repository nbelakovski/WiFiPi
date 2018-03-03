.. |ss| raw:: html

   <strike>

.. |se| raw:: html

   </strike>

WiFiPi
======

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution for easy remote setup, requiring no keyboard, monitor, mouse, and no modifications to the image.

Just throw this image, with NO modifications, onto an SD card, and pop it into your Pi. Your Pi is now broadcasting a WiFi network called WiFiPi-xxxx (where xxxx is the last 4 digits of the Pi's serial number),
password 'luggage12345'. Connect to it, at either ``ssh pi@wifipi-xxxx.local`` or ``ssh pi@10.0.0.5``. Default password is 'raspberry'.
Now you're in! Edit /etc/wpa_supplicant/wpa_supplicant-wlan0.conf with your network information, or just go write some code!

This repository contains the source script to generate the distribution out of an existing `Raspbian <http://www.raspbian.org/>`_ distro image.

Where to get it?
----------------

See the `releases tab <https://github.com/nbelakovski/WiFiPi/releases>`_

How to use it?
--------------
Terse Instructions
""""""""""""""""""

#. Write the image to an SD card
#. (optional) Edit the wpa_supplicant-wlan0.conf file located on the SD card. Remaining steps assume you have not done this.
#. Pop it into the Pi and boot it up
#. Connect to "WiFiPi-xxxx" where xxxx will be the last 4 digits of the Pi's serial number. Password is 'luggage12345'
#. ssh into the pi with either pi@wifipi-xxxx.local (same xxxx as the WiFiPi-xxxx), or pi@10.0.0.5. Password is 'raspberry'
#. Edit /etc/wpa_supplicant/wpa_supplicant-wlan0.conf with your network info.
#. After you've written to the file, you can run ``journalctl -f -u autohotspot.service`` and you should see autohotspot start within a minute or two and pick up your changes. Of course, your connection will break at that point (protip, you can exit an ssh session by typing ~. in your terminal), and you'll need to establish a new one once the Pi connects to your desired network.

|ss| Pedantic |se| Noob-friendly instructions
"""""""""""""""""""""""""""""""""""""""""""""

#. Write the image to an SD card. There's a few guides for how to do this, so I won't cover it here. `Here is the official guide.  <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Pop it into the Pi and boot it up. No need to connect a monitor or keyboard or anything like that.
#. On your laptop/desktop connect to WiFiPi-xxxx (you'll see different characters from xxxx but that's fine), password is luggage12345. You may want to write down what you see in place of xxxx.
#. If you're on Mac or Linux, open a terminal and type ``ssh pi@10.0.0.5``. If you're on Windows, you will need to download an SSH client. I hear PuTTY is a popular one. For setting that up and connecting to the Pi you'll have to look elsewhere for a guide.
#. Now you're connected to the Pi, hoorary! Pat yourself on the back!

From here, you can do whatever it is you wanted to do with the Pi. Most people will probably want it to connect to their local WiFi so that can download things from the internet. To do that, follow these steps:

#. From your terminal with the ssh connection, type ``nano /etc/wpa_supplicant/wpa_supplicant-wlan0.conf``.
#. Edit one of the network blocks to reflect your WiFi setup, and remember to remove the "#" characters at the beginning of the lines of the network block
#. Press Ctrl+O to save your changes, and Ctrl+X to exit. Within a minute (or possibly as soon as you hit Ctrl+O), the autohotspot service will pick up your changes and attempt to connect to the network you specified.
#. Your ssh connection may get stuck as autohotspot kills the WiFiPi-xxxx network to attempt to connect to the network you specified. If that's the case, type '~.' into your terminal and that should disconnect you.
#. If the Pi successfully connected to the network you specified, connect your laptop/desktop to the same network, and from your terminal type ``ssh pi@wifipi-xxxx.local`` where xxxx is what you wrote down in a previous step. If you don't remember it, you could go to your computer's saved WiFi networks and see if it's there, or use other tools to find the IP address of your Pi and connect to it via IP instead of hostname (i.e. ``ssh pi@123.456.789.012`` instead of ``ssh pi@wifipi-xxxx.local``) (perhaps logging into your router and seeing who's connected).
#. If it was unsuccessful, it should bring WiFiPi-xxxx back up, and from there you can ssh back into it and examine your network settings. If they look correct and you want it to try again, type ``touch /etc/wpa_supplicant/wpa_supplicant-wlan0.conf`` and wait a minute for autohotspot to try again.
#. If you want to take a look at logs to see if there's a hint as to why it was unsuccessful, try ``journalctl -u autohotspot`` and also ``journalctl -u wpa_supplicant@wlan0``

If you're having issues, come back here and create an issue in the Issues tab!


Features
--------

* `AutoHotspot <http://www.raspberryconnect.com/network/item/331-raspberry-pi-auto-wifi-hotspot-switch-direct-connection>`_ Automatically launch a Direct Connection WiFi hotspot when no other connection is available.
* `Raspbian <http://www.raspbian.org/>`_ tweaked for maximum preformance in a headless configuration

Developing
----------

For development instructions, see the corresponding section at https://github.com/guysoft/CustomPiOS

Code contribution would be appreciated!

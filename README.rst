WiFiPi
======

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution for easy setup, requiring no keyboard, monitor, mouse, and no custom image.

Just throw this image, with NO modifications, onto an SD card, and pop it into your Pi. Your Pi is now broadcasting a WiFi network called WiFiPi,
password 'luggage12345'. Connect to it, and either `ssh pi@wifipi.local` or `ssh pi@10.0.0.5` if that's not working. Default password is 'raspberry'.
Now you're in!

This repository contains the source script to generate the distribution out of an existing `Raspbian <http://www.raspbian.org/>`_ distro image.

Where to get it?
----------------

Releases tab

How to use it?
--------------

#. Unzip the image and install it to an sd card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Boot the Pi from the card
#. Connect to WiFiPi, password is luggage12345
#. Log into your Pi via SSH (it is located at ``wifipi.local`` `if your computer supports bonjour <https://learn.adafruit.com/bonjour-zeroconf-networking-for-windows-and-linux/overview>`_ or at 10.0.0.5), default username is "pi", default password is "raspberry".
#. Edit /boot/wifipi-wpa-supplicant.conf to add the information of the wifi network you'd like to connect to. Optionally, edit /boot/wifipi-network.txt if you'd like to specify additional configuration parameters, like static IP address and so forth. But make sure SSID and password information is in /boot/wifipi-wpa-supplicant.conf, since that is the file used by AutoHotspot to connect to WiFi.
#. AutoHotspot will run once per minute to check that the connection is still active, and launch a hotspot if it is not. You can disable it via ``sudo systemctl autohotspot disable``. You can see logs from AutoHotspot via ``journalctl -u autohotspot``.

Features
--------

* `AutoHotspot <http://www.raspberryconnect.com/network/item/331-raspberry-pi-auto-wifi-hotspot-switch-direct-connection>`_ Automatically launch a Direct Connection WiFi hotspot when no other connection is available.
* `Raspbian <http://www.raspbian.org/>`_ tweaked for maximum preformance in a headless configuration

Developing
----------

Requirements
~~~~~~~~~~~~

#. `qemu-arm-static <http://packages.debian.org/sid/qemu-user-static>`_
#. `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_
#. Downloaded `Raspbian <http://www.raspbian.org/>`_ image.
#. root privileges for chroot
#. Bash
#. git
#. realpath
#. sudo (the script itself calls it, running as root without sudo won't work)

Build WiFiPi From within WiFiPi / Raspbian / Debian / Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

WiFiPi can be built from Debian, Ubuntu, Raspbian, or even WiFiPi.
Build requires about 2.5 GB of free space available.
You can build it by issuing the following commands::

    sudo apt-get install gawk util-linux realpath qemu-user-static git p7zip-full python3
    
    git clone https://github.com/guysoft/CustomPiOS.git
    git clone https://github.com/nbelakovski/WiFiPi.git
    cd WiFiPi/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspbian_lite_latest'
    cd ..
    ../../CustomPiOS/src/update-custompios-paths
    sudo modprobe loop
    sudo bash -x ./build_dist
    
Building WiFiPi Variants
~~~~~~~~~~~~~~~~~~~~~~~~

WiFiPi supports building variants, which are builds with changes from the main release build. An example and other variants are available in [CustomPiOS, folder ``src/variants/example``](https://github.com/guysoft/CustomPiOS/tree/CustomPiOS/src/variants/example).

To build a variant use::

    sudo bash -x ./build_dist [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build WiFiPi in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

Make sure you have a version of vagrant later than 1.9!

If you are using older versions of Ubuntu/Debian and not using apt-get `from the download page <https://www.vagrantup.com/downloads.html>`_.

To use it::
    
    sudo apt-get install vagrant virtualbox
    git clone https://github.com/nbelakovski/WiFiPi.git
    git clone https://github.com/guysoft/CustomPiOS.git    
    cd WiFiPi/src
    ../../CustomPiOS/src/update-custompios-paths
    cd WiFiPi/src/vagrant
    vagrant up
    run_vagrant_build.sh

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd WiFiPi/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd src/vagrant
    run_vagrant_build.sh [Variant]
    

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/modules/wifipi/config``. If you need to override the path to the Raspbian image to use for building WiFiPi, override the path to be used in ``ZIP_IMG``. By default the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created at the ``src/workspace``

Code contribution would be appreciated!

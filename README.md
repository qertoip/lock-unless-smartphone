# lock-unless-smartphone
Auto lock screen if your smartphone is not connected via USB.

Tested on Arch Linux + Cinnamon but should be trivial to accommodate for any Linux distribution.

## Boost your physical opsec

One of the most likely and severe security fuckups is simply forgetting to lock your desktop.
The time-based locking is far not enough in most environments.
Ideally, desktop should lock immediately when you depart from the computer.
These simple scritps implement exactly that: locking the screen out when you disconnect the smartphone.

## Assumptions

* You always get your smartphone with you when you leave your computer.

* You always connect your smartphone back with the USB cable when you come back to your computer (for charging, etc).

## Instructions

Connect your smartphone with USB cable.

Learn your smartphone serial number when connected as USB device:

    lsusb      # list all USB devices
    lsusb -v   # list all USB devices with full details

Create `/etc/lock-unless-smartphone.conf` with the serial number of your smartphone:

    #!/bin/bash

    export USB_DEVICE_SERIAL_NUMBER=XDE1231423   # replace with your smartphone S/N

Symlink *.service and *.timer files to /usr/lib/systemd/system/ respectively.

Enable and start the timer:

    sudo systemctl enable lock-unless-smartphone.timer
    sudo systemctl start lock-unless-smartphone.timer

Test the setup by disconnecting the smartphone cable. The desktop should lock in under 4 seconds. If not, debug with `journalctl -r`, `systemctl list-timers`, `systemctl status lock-unless-smartphone.timer` etc.

I was able to contact a Teensy person that was able to help. The first screenshot on the page that was linked was incorrect, it should be 

echo 'SUBSYSTEM=="usb_device", SYSFS{idVendor}=="16c0", SYSFS{idProduct}=="0478", MODE="0666"' > /tmp/49-teensy.rules

but actually there are easier instructions that can be followed which also include using the recommended settings by PJRC. The entire discussion is here https://forum.pjrc.com/threads/45595-Not-able-to-update-firmware-on-Ergodox-keyboard-on-OpenSuse-42-3-Leap?p=150477

but the main gist of it is that these commands could be used to get the teensy tool working

To download the official 49-teensy.rules from pjrc.com and install into /lib/udev/rules.d/ ,
Code:
( sudo rm -f /tmp/49-teensy.rules /etc/udev/rules.d/49-teensy.rules /lib/udev/rules.d/49-teensy.rules &&   wget -O /tmp/49-teensy.rules https://www.pjrc.com/teensy/49-teensy.rules &&    sudo install -o root -g root -m 0664 /tmp/49-teensy.rules /lib/udev/rules.d/49-teensy.rules &&   sudo udevadm control --reload-rules &&   sudo udevadm trigger &&   echo "Success" )
As an explanation, the first line removes the existing rules file, if any. The second line uses wget to download the rules from pjrc.com. The third line copies it to /lib/udev/rules.d, setting the owner, group, and mode to safe and sane values. The next two lines (one line in the earlier version) tells udev, the device monitoring service, to reload its rules, and make them effective immediately. The final line is only run if all the previous lines succeeded, and prints Success. so the user knows it worked.

The installation instructions at the Teensy Loader / Linux page does not explicitly tell you how to download the rules file, and only tells you to install it with sudo cp 49-teensy.rules /etc/udev/rules.d/ . That works too, but I prefer to use the install utility instead

Hat tip to **Garvin Guan** for the excellent research above!

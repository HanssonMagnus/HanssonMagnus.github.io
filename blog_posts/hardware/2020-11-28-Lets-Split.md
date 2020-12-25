---
layout: post
title: Let's Split Keyboard [diy]
---

Keyboard: Let's Split

Switches: Gazzew Bobba U4 68g (w/ Super Lube PTFE and films)

Keycaps: ePBT Blanks

## Images
<img src="/images/ls_1.jpg" width="500"> <img src="/images/ls_2.jpg" width="500">

## Parts

* PCB by /u/wootpatoot. (I bought mine at
  [mechboards.co.uk](https://mechboards.co.uk/shop/kits/lets-split-kit/#pcb) )
* 48 diods.
* 2 TRRS jacks.
* 48 switches.
* 2 micro controllers.
* A case/plate.

I cannot recommend [Splitkb.com](https://splitkb.com/) and
[Mechboards.co.uk](https://mechboards.co.uk/) enough. I haven't used so many other vendors but
these are fantastic (both located in Europe).

## Building the Let's Split

There is a very good [guide](https://github.com/nicinabox/lets-split-guide) to building the
keyboard, which is recommended to follow. It covers everything in detail except flashing the micro
controllers.

The first thing to do is to solder on diodes, TRRS jacks, headers, and connect jumpers.

<img src="/images/ls_3.jpg" width="500">

Next it is recommended to test the controllers by plugging them in and checking with a multimeter.
E.g., the Pro Micro should have around 5V between VCC and RAW. Now is also a good time to flash the
firmware onto the controllers.

After this is done what is left is to assemble everything.

## Flashing the micro controllers
When building the keyboard I chose to use one Pro micro and one Elite-C micro controller. This is a
common choice, since the Elite-C is an upgrade, but unnecessary to have on both halves. One thing
to keep in mind is that the two controllers have different bootloaders, and this has to be taken
into account when flashing.

When flashing, QMK only builds files with name keymap.c. In the make command the 'keymap' is the
name of the folder containing the keymap.c file. This means that when you write,

```
make KEYBOARD:YOUR_KEYMAP_NAME:BOOTLOADER
```
this should be done from the top level qmk_firmware directory. See example below for each
controller.


There are a few things to keep in mind when flashing the halves,

* Reset the micro controllers right after you entered the flash command in the CLI.

* Flash the same keymap.c file to both sides.

* To operate on a device you need permission. Thus sudo or udev rules are necessary.

* On some Linux systems people have had problems flashing when Modem Manager is active. I had this
  problem, and eventually found
  [this](https://www.reddit.com/r/olkb/comments/8k3tr9/help_flashing_viterbi/) Reddit thread.

Before you flash the controllers, copy the default firmware to the new build with,
```
qmk new-keymab -kb lets_split
```

Now you have a keymap.c file in ~/qmk_firmware/keyboards/lets_split/keymap/HanssonMagnus and a
config.h file. You can edit these files as you please.

Stop the modem manager if this is an issue for you,
```
systemctl stop ModemManager.service
```

Compile the keymap,
```
sudo make lets_split/rev2:HanssonMagnus
```

#### Pro micro

Flash the Pro Micro with avrdude,
```
sudo make lets_split/rev2:HanssonMagnus:avrdude
```

The CLI will tell you to reset the controller, this is done by shorting the controller with a
pair of tweezers (or similar), you short the RST and GND. When the flash is done you will get the
following message,

```
avrdude: safemode: Fuses OK (E:FD, H:D8, L:FF)

avrdude done.  Thank you.
```

#### Elite-C
The Elite-C flashing process is similar to the Pro Micro, reset the controller with the reset
button on the controller.
```
sudo make lets_split/rev2:HanssonMagnus:dfu
```

The Elite-C with dfu will not give you a message when it is done, it will just say,

```
Bootloader Version: 0x00 (0)
Validating...
18294 bytes used (63.80%)
```

## Additional information
After I assembled the keyboard I recognized that I if I wanted to have the TRRS jacks next to each
other and not on the outside of the boards, I had to use the Pro Micro as my master. Since I
in any case wanted to change a detail in my keymap I decided to change so that the Elite-C worked
as the master. This was done by changing "#define MASTER_LEFT" to "#define MASTER_RIGHT" in the
config.h file, in the same directory as my keymap.c file.

It worked completely fine to short reset the Pro Micro with tweezers when it was soldered.

## What I will do different in the future

* Maybe socket the micro controllers to hedge if something were to go wrong.

* Maybe install a reset button if I use Pro Micro, especially if I have a different case that is
  more work to take apart.


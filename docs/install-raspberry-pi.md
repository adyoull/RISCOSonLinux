# Installing on a Raspberry Pi

**Works on:** Raspberry Pi 3 B / 3 B+ / CM3, Zero 2 W, 4 / 400 / CM4 and
5 / 500 / CM5. RISC OS runs natively on all of them.

## What you need

- `riscos-generic.img.xz` from [Releases](../../../releases);
- a micro SD card or USB stick of 4 GB or more;
- a USB keyboard and mouse, and an HDMI screen;
- wired network (Wi-Fi can be set up later, in Configure).

## Writing the card

1. Open [Raspberry Pi Imager](https://www.raspberrypi.com/software/).
2. Choose your Pi, then "Use custom" as the operating system, and pick `riscos-generic.img.xz`.
3. Choose the card and write it. Don't change any of Imager's "OS customisation" settings: the image doesn't use them.

balenaEtcher works too. So does `xz -d` followed by `dd`.

## Starting

Put the card in and switch on.
- The first start takes a little longer: the Linux partition is enlarged to fill the card, so HardDisc4 gets all the space.
- The start-up screen should say "Starting RISC OS (native)...".
- RISC OS then starts at the monitor's own resolution.

A card that also has U-Boot for a Pinebook still starts on a Pi.

## Wi-Fi

Pi 3 B, 3 B+, Zero 2 W, 4, 5, 500, CM4 and CM5: Configure → **Linux network**.
Choose Wi-Fi, pick the network and type its password.

## Sound

Sound plays through HDMI if the screen takes sound, otherwise through the
headphone socket. A USB sound card comes first when one is plugged in. Set
the volume in Configure → Sound.

## Next

- [Using RISC OS on Linux](using.md)
- [Updates and going back](updates-and-rollback.md)
- [Troubleshooting](troubleshooting.md)

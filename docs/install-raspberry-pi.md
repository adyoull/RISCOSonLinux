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

### Compute Module 5, Pi 500 and newer Pi 5 boards

The 8 Nov 2026 image (1.0.0020) lacks four files and a setting:

- `overlays/bcm2712d0.dtbo`, which the Compute Module 5, the Pi 500 and
  the newer Pi 5 boards (those with the "D0" version of the processor)
  need. Without it Linux stops half a second after starting with "Kernel
  panic - not syncing: Asynchronous SError Interrupt".
- `bcm2712-rpi-cm5l-cm5io.dtb` and `bcm2712-rpi-cm5l-cm4io.dtb`, which a
  Compute Module 5 Lite (the CM5 without built-in storage, started from
  an SD card or NVMe) needs. Without them the Pi says "the installed
  operating system does not indicate support for the Raspberry Pi 5".

- `overlays/dwc2.dtbo` and some lines in `config.txt`, which any Compute
  Module (4 or 5) needs for the USB sockets of its IO board. Without them
  the keyboard and mouse do nothing, USB drives are ignored, and a card
  image on a USB drive starts to a black screen. (The module's own USB
  port starts as a device port; the lines make it a host, as Raspberry
  Pi OS's `config.txt` does.)

To add them to a card:

1. Download [cm5-boot-files.zip](files/cm5-boot-files.zip) on another computer and unzip it.
2. Open the card's first partition (the one other computers can read).
3. Copy the two `bcm2712-rpi-cm5l-*.dtb` files onto it, next to the other `bcm2712-*.dtb` files.
4. Copy `bcm2712d0.dtbo` and `dwc2.dtbo` into its `overlays` folder.
5. Open the card's `config.txt` in a text editor and add the lines of
   `add-to-config.txt` (from the zip) at the end:

   ```
   [cm4]
   otg_mode=1
   [cm5]
   dtoverlay=dwc2,dr_mode=host
   [all]
   ```
6. If you added `os_check=0` to `config.txt`, take it out.

This has not been tried on these boards here: please
[report](https://github.com/adyoull/RISCOSonLinux/issues) how it goes.
Systems updated to `riscos-kernel-pi` 6.6.78-13 have all of this (the
update makes the `config.txt` change itself), and later images will. ([cm5-lite-dtbs.zip](files/cm5-lite-dtbs.zip) is the earlier
download, with the two `.dtb` files only.)

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

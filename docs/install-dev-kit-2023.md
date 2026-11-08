# Installing on the Windows Dev Kit 2023

The Dev Kit's Snapdragon 8cx Gen 3 runs 32-bit ARM programs, so RISC OS
runs natively. Use **`riscos-wdk2023.img.xz`**, not the generic image: the
Dev Kit needs its own device tree.

The Dev Kit image is **only** for the Dev Kit. Anywhere else (UTM, QEMU, a
Pi) Linux finds no screen, and the machine switches off a minute later.

## What you need

- `riscos-wdk2023.img.xz` from [Releases](../../../releases);
- a USB stick or USB SSD of 4 GB or more;
- a screen on the mini DisplayPort socket, and a USB keyboard and mouse on
  the USB-A ports.

Windows on the Dev Kit's own SSD is left alone.

## Writing the stick

Raspberry Pi Imager ("Use custom"), balenaEtcher, or `xz -d` then `dd`.

## Starting it

1. In Windows: **Settings → System → Recovery → Advanced startup → Restart
   now**, then **Troubleshoot → Advanced options → UEFI Firmware Settings**.
2. In the firmware's settings:
   - set **Secure Boot** to **None**. The kernel isn't signed by Microsoft, and "Microsoft and 3rd party CA" isn't enough;
   - **enable USB boot**;
   - put **USB first** in the boot order.
3. Plug the stick into a **USB-A** port (not USB-C) and restart.
   - A stick in a USB-C port still starts, but the DSPs stay off, so there are no USB-C devices: starting them resets the USB-C ports and would take the stick away.

To go back to Windows, take the stick out. Turn Secure Boot back on if you
want it.

## Network

The wired Ethernet port, with DHCP. Configure → Linux network sets it.
Wi-Fi isn't supported on this machine yet.

## Next

- [Using RISC OS on Linux](using.md)
- [Updates and going back](updates-and-rollback.md)
- [Troubleshooting](troubleshooting.md)

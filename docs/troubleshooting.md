# Troubleshooting

## A dark screen, or it switches off

Every start copies its logs to the card's FAT partition, which any computer
can read.

1. Start the machine and wait two minutes.
2. Switch off.
3. Look in the FAT partition's **`LOGS`** folder:
   - `riscos-log.txt` and `kernel-log.txt`: the last start;
   - `*.prev.txt` and `previous-end.txt`: the start before, and how it ended.

Please attach these to an [issue](../README.md#reporting-a-problem).

**On the Windows Dev Kit:** the image only works on the Dev Kit, and
anywhere else it switches off after a minute. On the Dev Kit itself,
check that the USB stick is in a USB-A port and Secure Boot is set to
None ([installing](install-dev-kit-2023.md)).

**On UTM:** see [the 4 November problem](install-utm-mac.md#already-have-an-older-utm-machine-that-wont-start).

## A new RISC OS won't start

Put the card in any computer and make an empty file called **`ROLLBACK`** on
its small FAT partition. The next start puts back the RISC OS that was
replaced. See [Updates and going back](updates-and-rollback.md).

## A new kernel won't start

Rename the `*.old` file on the FAT partition back:
- `EFI/BOOT/BOOTAA64.EFI.old` → `EFI/BOOT/BOOTAA64.EFI`;
- on a Pi, `kernel8.img.old` → `kernel8.img`.

## Forgotten root password

A file called **`RESETPW`** on the small FAT partition of the disc RISC OS
on Linux starts from clears the root password at the next start, and is
then removed. The next `*Linux`, Updates or RiSH root terminal asks you to
choose a new one. The file only has to exist (`RESETPW.TXT` works too, and
it needn't be empty).

It can't be made from inside RISC OS on Linux without the password you have
forgotten: that is deliberate. Make it from another computer or another
system, whichever suits the machine. (To change a password you still know,
use `*Linux passwd` instead.)

### An SD card, USB stick or USB drive (any machine)

Put it in any other computer. The FAT partition appears as a small disc
with `config.txt` and `kernel8.img` on it (or an `EFI` folder). Make the
file there:

- **Mac:** in Terminal, `touch /Volumes/NAME/RESETPW`, with the disc's name
  as Finder shows it.
- **Windows:** in the disc's window, New → Text Document, and name it
  `RESETPW` (`RESETPW.txt` is fine).
- **Linux:** `touch /media/$USER/NAME/RESETPW`, or wherever it is mounted.

Eject it, put it back and start the machine.

### A Raspberry Pi with RISC OS on Linux on an internal NVMe drive

Start the Pi from an SD card with Raspberry Pi OS on it (the Pi starts from
the SD card first unless its boot order was changed). Then, in a terminal:

```
lsblk
sudo mkdir -p /mnt/fat
sudo mount /dev/nvme0n1p1 /mnt/fat
ls /mnt/fat
sudo touch /mnt/fat/RESETPW
sudo umount /mnt/fat
```

`lsblk` lists the drives: use the first partition of the one RISC OS on
Linux is on, if it isn't `nvme0n1p1`. `ls` should show `config.txt` and
`kernel8.img`. Shut down, take the SD card out and start again.

### Windows Dev Kit 2023 and other UEFI machines, on an internal drive

The FAT partition is the drive's first partition (it holds the `EFI`
folder). Either:

- **start from a Linux live USB stick** (any 64-bit ARM one that starts on
  the machine) and use the commands above, with the internal drive's first
  partition (`lsblk` shows it; usually `nvme0n1p1`); or
- **in the UEFI Shell**, if the firmware has one:

  ```
  map
  FS0:
  ls
  echo x > RESETPW
  ```

  `map` lists the file systems; pick the one whose listing shows the `EFI`
  folder and RISC OS on Linux's files, if it isn't `FS0:`.

### Pinebook and Pinebook Pro, on the internal eMMC

Start from an SD card with any Linux for the machine (the Pinebook starts
from the SD card first; the Pinebook Pro does with most U-Boots). Then:

```
lsblk
sudo mkdir -p /mnt/fat
sudo mount /dev/mmcblk2p1 /mnt/fat
sudo touch /mnt/fat/RESETPW
sudo umount /mnt/fat
```

The eMMC is the `mmcblk` device that isn't the SD card you started from
(`lsblk` shows their sizes); use its first partition.

### UTM on a Mac

Shut the virtual machine down. In its settings add a CD/DVD drive with a
64-bit ARM Linux live ISO (Debian's or Ubuntu's), put it first in the boot
order, and start it. In the live system's terminal:

```
lsblk
sudo mkdir -p /mnt/fat
sudo mount /dev/vda1 /mnt/fat
sudo touch /mnt/fat/RESETPW
sudo umount /mnt/fat
```

Use the virtual disc's first partition if it isn't `vda1`. Shut down,
remove the CD drive from the settings and start the virtual machine
normally.

These follow from how the reset works (it looks at partition 1 of the disc
the system starts from). The removable-disc way is the one that has been
tried; if one of the others doesn't go as written on your machine, please
say.

## The disc didn't grow to fill the card

At each start the Linux partition is enlarged to fill the card or disc, if
there is room. An empty file called `NOGROW` on the FAT partition stops
that: delete it if you want the partition enlarged.

## RISC OS stops or a program crashes

- **Ctrl+Break** stops a hung RISC OS, and logs where it was.
- **For a detailed log,** run `*Create <IXFS$HardDisc4>.NativeDebug` and restart.
  - NativeSupport's fault reports then go into the start-up log: `IXFS:$.riscos.log/txt` in RISC OS, also copied to the card's `LOGS`.
  - Delete the file again afterwards.
- **`*NativeSupport`** shows what NativeSupport has done.

Please [report](../README.md#reporting-a-problem) what you saw, with the
versions that Configure → Updates shows.

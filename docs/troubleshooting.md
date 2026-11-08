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

Put the card in any computer and make an empty file called **`RESETPW`** on
its small FAT partition. The next start clears the root password and removes
the file. The next `*Linux` or Updates asks you to choose a new one.

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

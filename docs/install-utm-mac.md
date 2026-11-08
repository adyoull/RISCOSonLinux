# Installing on an Apple Silicon Mac (UTM)

Apple's processors don't run 32-bit ARM programs, so on a Mac RISC OS runs
under QEMU. That is slower than on a Pi, but fine for the desktop and most
programs.

## What you need

- [UTM](https://mac.getutm.app) (free);
- `RISC OS 1.0.0019.utm.zip` from [Releases](../../../releases).

## Installing

1. Unzip `RISC OS 1.0.0019.utm.zip`.
2. Double-click `RISC OS 1.0.0019.utm`. UTM adds it to its list.
3. Start it. RISC OS starts at the window's size. Resizing the window changes
   the screen mode to fit, for that session.

The virtual disc is 32 GB. It grows to that size at the first start, but
only uses the Mac's disc space as files are added.

## Already have an older UTM machine that won't start?

The 4 November updates installed RISC OS 5.31-16 and 5.31-17 so that only
the Linux root account could read them. Under QEMU, RISC OS then stopped at
once: the start-up screen showed, and the machine switched off a minute
later. RISC OS 5.31-17.1 fixes it, but a machine in that state can't start
RISC OS to run Updates.

Start again from the newest `.utm`. If you have files on the old machine,
keep it and [ask](../README.md#reporting-a-problem): they can be copied out.

## Next

- [Using RISC OS on Linux](using.md)
- [Updates and going back](updates-and-rollback.md)

# RISC OS on Linux

RISC OS 5 on 64-bit ARM machines, running on a small Debian 13 Linux that
starts straight into the RISC OS desktop. One card image works on a
Raspberry Pi 3, 4, 5 or Zero 2 W, a Pinebook or Pinebook Pro, and other
UEFI machines. A ready-made virtual machine covers Apple Silicon Macs (UTM),
and there is a separate image for the Windows Dev Kit 2023.

Where the processor can run 32-bit ARM programs (every Raspberry Pi above,
the Pinebooks, the Dev Kit) RISC OS runs natively, at full speed. Elsewhere
(an Apple Silicon Mac, most ARM servers) it runs under QEMU.

![The desktop with NetSurf](docs/screenshots/1.0.0017/01-desktop-netsurf.png)

This is a **test release**: it runs well on the machines below, but please
[report anything that doesn't work](#reporting-a-problem).

## Download

Get the images from **[Releases](../../releases)**. The newest is
[1.0.0020](releases/1.0.0020.md), the first public release (with [the full history](releases/1.0.0020.md#the-full-history) of how it got here).

| Image | For | File |
|---|---|---|
| Generic | Raspberry Pi 3/3B+/CM3, Zero 2 W, 4/400/CM4, 5/500/CM5; Pinebook and Pinebook Pro; UEFI machines (QEMU, ARM boards and servers) | `riscos-generic.img.xz` |
| UTM | Apple Silicon Macs | `RISC OS 1.0.0020.utm.zip` |
| Windows Dev Kit 2023 | Snapdragon 8cx Gen 3 ("Project Volterra") only | `riscos-wdk2023.img.xz` |

`SHA256SUMS` in each release lists the files' checksums.

## Installing

- [Raspberry Pi](docs/install-raspberry-pi.md)
- [Apple Silicon Mac (UTM)](docs/install-utm-mac.md)
- [Windows Dev Kit 2023](docs/install-dev-kit-2023.md)
- [Pinebook, Pinebook Pro and other UEFI machines](docs/install-uefi-and-pinebook.md)

In short: write the `.img.xz` to a card or USB stick of 4 GB or more with
[Raspberry Pi Imager](https://www.raspberrypi.com/software/) ("Use custom")
or [balenaEtcher](https://etcher.balena.io), and start the machine from it.
On a Mac, unzip the `.utm` and double-click it.

## Using it

[Using RISC OS on Linux](docs/using.md) covers:
- the screen, sound and network;
- USB discs and SD cards;
- the `*Linux` command and the root password;
- shutting down.

## Keeping it up to date

**Configure → Updates** keeps everything up to date without reinstalling:
RISC OS itself, its Linux side, the kernel and Debian's security fixes. You
can also roll RISC OS back to the build before. See
[Updates and going back](docs/updates-and-rollback.md); what each update
changed is listed in Updates' **Changes** window.

## If something goes wrong

[Troubleshooting](docs/troubleshooting.md) covers:
- a dark screen, and the start-up logs every machine writes to its card;
- a new RISC OS or kernel that won't start;
- a forgotten root password;
- UTM machines that stopped starting after the 4 November update.

## Reporting a problem

Open an [issue](../../issues/new/choose). Please include:
- the machine;
- the image build;
- the versions Configure → Updates shows;
- what happened;
- the logs from the card's `LOGS` folder, if the problem is at start-up.

## Known issues

- The Filer can stop with an abort in the kernel's sprite plotting when
  opening certain directories (seen with !RiscOSM's).
- On a Raspberry Pi, ROBench's BPut/BGet tests fail to open their file.
- Discs inside the machine in RISC OS's own format (FileCore on an internal
  SSD or NVMe) aren't offered to RISC OS. USB discs and SD cards are.
- Socket_Connect can report success for a connection that was refused.
- Iris isn't included. Install it with PackMan, which is already set up
  with riscosdev.com's list. (If PackMan says "One or more files conflict",
  run Configure → Updates first: riscos-harddisc 1.0.0040-1 fixes it.)
- The Raspberry Pi 5's fan doesn't start on 1.0.0020 as downloaded:
  Configure → Updates brings the kernel that drives it (6.6.78-7).
- Full-screen Linux programs (nano, vi, top) don't work from `*Linux` or
  `*IXRun` yet.

## Built on Timothy Baldwin's port

RISC OS on Linux is built on Timothy Baldwin's
[Linux port of RISC OS](https://github.com/TimothyEBaldwin/RISC_OS_Linux_Source).
It made it possible:
- the RISC OS "ROM" as a Linux program;
- its HAL on Linux's system calls;
- IXFS;
- the SDL front end;
- the sandbox and the build.

This is his to-do list, from his README, and where each item stands now:

| His to-do item | Status |
|---|---|
| Complete the key map | Done by Timothy |
| Fix the realtime clock | Done by Timothy |
| Make data aborts raise errors | Done by Timothy |
| Support writing timestamps in IXFS | Done by Timothy |
| Support reading timestamps in IXFS to centisecond resolution | Done by Timothy |
| Test timestamps | Done by Timothy |
| Add sound modules | Done by Timothy |
| **Add sound output** | **Done here:** RISC OS Open's sound modules play through Linux (HDMI, headphones, USB) |
| Officially allocate names and SWI numbers etc. | Done by Timothy (and RISC OS Open allocated ours for the new modules) |
| Fix failure to truncate unbuffered files opened for output, and the race | Done by Timothy |
| Fix spurious FSEntry_Args 6 (flush) calls when files are closed | Done by Timothy |
| **Perform proper error handling in IXFS** | **Done here:** RISC OS's own errors, the Locked attribute, files over 2 GB |
| Split the video, keyboard and mouse driver into a separate module | Done by Timothy |
| **Detect screen updates to avoid copying the full screen 20 times a second** | **Done here:** only what changed is redrawn |
| **Finish the video, keyboard and mouse driver** | **Done here, as a new front end:** rol-fb, on Linux's KMS (the monitor's own modes, hardware pointer, keyboard LEDs, scroll wheel, video overlay) |
| Fix overwriting of the ROM image when rebuilding RISC OS while it's running | Not addressed for building (Updates replaces the ROM safely on a running machine) |
| **Provide a means to run Linux programs** | **Done here:** `*Linux` (as root, with a password), `*LinuxUser` and `*IXRun` |
| **Implement PMP dynamic areas** | **Done here:** in RISC OS's kernel (5.31-3), so ARMEABISupport 1.08 and Iris work |
| Replace the changes labelled "Hack" | Done by Timothy |
| Replace the branch table at the beginning of the HAL | Done by Timothy |
| **Write test cases for VFP errors** | **Partly:** RISC OS Open's VFPSupport tests and ours; over 20 million floating point and NEON results checked bit for bit |
| **Implement VFPSupport** | **Done here:** everything RISC OS Open's VFPSupport 0.18 does that the port allows |
| **Possibly add a binary release** | **Done here:** these images, and a signed package repository for Updates |

**And beyond his list:**
- **A complete system:**
  - it starts straight into the desktop on Raspberry Pis, UEFI machines,
    the Windows Dev Kit 2023, the Pinebooks and Apple Silicon Macs, on
    Debian 13;
  - Updates and going back;
  - protected RISC OS.
- **RISC OS 5.31:** RISC OS Open's current sources merged into the port,
  which was 5.29, with 18 builds since.
- **NativeSupport:** programs run as they were compiled, natively. It
  patches privileged instructions into user mode ones on the fly while
  keeping programs compatible, so the DDE still compiles valid binaries.
- **VFP and NEON** for RISC OS programs (VFPSupport, which his port
  didn't have yet).
- **Speed**, against RISC OS on Linux before each change. Native RISC OS
  is still faster at most things; [the measured figures](docs/what-we-added.md#speed-measured) give
  native RISC OS's times too.
  - Natively on a Pi 4, SWIs are about 3× faster: 2.4 µs → 745 ns. Native
    RISC OS takes 162 ns.
  - Natively, FPA instructions are 4.5× faster.
  - Under QEMU on a Mac, SWIs are about 4.9× faster than with Timothy's QEMU
    changes alone.
  - Under QEMU, NEON is 1.9 to 4.7× faster than QEMU's standard emulation.
- **Hardware:**
  - networking and Wi-Fi;
  - USB discs and SD cards, including RISC OS's own format;
  - phones and cameras (PDFS);
  - the Pi's hardware video decoding and overlay;
  - GPIO;
  - the scroll wheel.
- **Programs:** NetSurf, RDPServer and Folio, and PackMan set up with
  RISC OS Open's lists, riscosdev.com's and riscos-packages.

Every item, in full: [What RISC OS on Linux adds to his port](docs/what-we-added.md).

## Where it comes from

- **RISC OS itself** is RISC OS Open's, in Timothy Baldwin's
  [Linux port](https://github.com/TimothyEBaldwin/RISC_OS_Linux_Source).
  [What RISC OS on Linux adds to his port](docs/what-we-added.md), item
  by item against his to-do list.
- **The images' own parts and how they're built:**
  [riscos-linux](https://github.com/adyoull/riscos-linux).
- **The packages Updates installs:**
  [riscos-rolpkg](https://github.com/adyoull/riscos-rolpkg).
- **Licences:** each image has its licences and where to get the sources in
  `LICENCES` and `SOURCE.txt` on its FAT partition, and in
  HardDisc4.Documents.

Thank you to **Chris Gransden** for testing and reports.

Copyright (c) 2026 Andrew Youll. These documents: MIT licence (see LICENSE).

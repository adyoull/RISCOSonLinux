# Pinebook, Pinebook Pro and other UEFI machines

Use **`riscos-generic.img.xz`**, the same image as for a Raspberry Pi.

## UEFI machines and virtual machines

The machine's firmware starts `EFI/BOOT/BOOTAA64.EFI` from the image's FAT
partition. That covers:
- QEMU and Parallels. On an Apple Silicon Mac, [UTM's ready-made machine](install-utm-mac.md) is easier;
- ARM boards and servers with standard UEFI firmware (ACPI, or a device tree from the firmware).

For a virtual machine, unpack the image (`xz -d`) and use the `.img` as its
disc. The screen is the firmware's (the EFI framebuffer), or virtio-gpu in
a virtual machine.

RISC OS runs natively if the processor can run 32-bit ARM programs.
Otherwise it runs under QEMU: Apple's processors and many ARM servers'
cores can't. To choose, put a file `/riscos/native` on the Linux partition:
- `off`: always run under QEMU;
- `on`: always run natively;
- `auto` (or no file): decide by itself.

## Pinebook and Pinebook Pro

These haven't been tried on the machines yet: treat them as experimental.

They have no UEFI firmware of their own, so U-Boot starts the image, as for
Debian and Ubuntu. The U-Boot is Ubuntu's (2025.10). It's in the `u-boot`
folder of the image's FAT partition.

1. Write the image to the SD card first.
2. Write that machine's U-Boot into the gap before the first partition
   (nothing else is touched). On a Mac, with the card as `/dev/diskN`
   (`diskutil list`, then `diskutil unmountDisk /dev/diskN`), from the
   card's RISCOS volume:

   **Pinebook (A64):**
   ```
   sudo dd if=u-boot/pinebook/u-boot-sunxi-with-spl.bin of=/dev/rdiskN bs=1k seek=8 conv=notrunc
   ```
   **Pinebook Pro (RK3399):**
   ```
   sudo dd if=u-boot/pinebook-pro/u-boot-rockchip.bin of=/dev/rdiskN bs=512 seek=64 conv=notrunc
   ```
   On Linux, use `of=/dev/sdX` or `/dev/mmcblk0`, with the same numbers.

A card can carry one of the two, not both: they overlap. A card with either
still starts on a Raspberry Pi or a UEFI machine.

**Which U-Boot runs:**
- **Pinebook:** it starts from the SD card ahead of its eMMC.
- **Pinebook Pro:** it starts from SPI flash or eMMC first. If that has a recent U-Boot (Tow-Boot, or Manjaro's, Armbian's or Debian's), it usually finds the card's `EFI/BOOT/BOOTAA64.EFI` by itself, and the `dd` above isn't needed.

**What works:** the internal screens, HDMI, SD and eMMC, USB (the keyboard and
touchpad are USB), sound, battery and charging, and Wi-Fi (Configure → Linux
network).

**Not supported:** the Pinebook Pro's USB-C video.

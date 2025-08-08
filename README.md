# Arch + Omarchy installation steps (so I don't forget)

## General steps

1. Download Arch Linux [Arch Linux ISO](https://mirrors.atlas.net.co/archlinux/iso/2025.08.01/archlinux-2025.08.01-x86_64.iso).


2. If installing Arch shrinking a disk, Disk Management (in Windows) can be used. Shrink the disk and leave it
   unallocated.


3. Create a bootable USB with [BalenaEtcher](https://etcher.balena.io/) and boot into Arch installer.


4. In Arch installer:

- List disks and identify the main disk which was shrunk:

```
lsblk
```

- Use the following tool to see the **free space** available (nvme0n1):

```
cfdisk /dev/nvme0n1
```

- On the free space, select "[new]" and type "512M" for the boot partition -> select "[type]" as EFI System.

- Select "[new]" again and create the root partition (/) for the rest of the space left (as linuxsystem).

- Select "[write]" -> type "yes" -> select "[quit]".

- Type ```lsblk``` and identify the names for the /boot (nvme0n1p5) and root / (nvme0n1p6) partitions.


5. Execute archinstall script. Follow [Omarchy's installation settings](https://manuals.omamix.org/2/the-omarchy-manual/50/getting-started).

```
archinstall
```


6. Locales -> keyboard layout: ```la-latin1```.


7. Mirrors and repositories -> select current country.


8. Disk configuration -> Partitioning -> Manual Partition:

- Select the shrunk disk (nvme0n1).

- (SKIP THIS STEP. ITWORKED ON THE DELL LAPTOP. DID NOT WORK FOR THE DESKTOP PC). EFI partition (100MB): set
  mountpoint to ```/boot/efi```. Format: ```fat32``` as default (do not format).

- Boot partition (512 MB): set mountpoint to ```/boot```. Format: ```fat32```.
- Root partition (rest of storage). Format: ```btrfs```.
    - Mark as compressed.
    - Creat a subvolume called ```root``` and set mountpoint to ```/```.
- Confirm and exit.
- Disk encryption -> type LUKS -> set password -> select the "btrfs" partition (```/```).


9. Bootloader -> ```grub```.


10. Audio -> ```pipewire```.


11. After Arch is installed, enter "chroot" (see [tutorial](https://youtu.be/xArcL6WVmwI?t=473)). Execute the
    following commands in Arch root:

```
sudo pacman -S efibootmgr grub mtools dosfstools
```

Type ```lsblk``` to see validate the /boot partition (fat32). Then execute the following:

```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```

- type the command "exit" -> type "reboot".


12. Change boot priority to GRUB:

- While rebooting, press the F2-key to enter the BIOS -> select the GRUB bootloader.
- If the bootloader loads the USB, press in the keyboard ```ctrl+alt+supr``` and enter the BIOS.


13. Login into Arch. Steps to update GRUB to recognize windows:

- Run again the following and check if Windows gets recognized:

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

- If Windows is not recognized, run:

```
sudo pacman -S nano
```

```
sudo nano /etc/default/grub
```

- Go to the end of the file, and remove the comment "#" from the line ```#GRUB_DISABLE_OS_PROBER=false```.

- Save the file with -> ctrl+O -> enter -> crtl+x to exit.

- Install os-prober and fuse3:

```
sudo pacman -S fuse3
```

- Execute the following again. The result should find the Windows boot loader:

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```


## Useful videos

1. [Dual Boot Arch Linux Hyprland & Windows 11 | NVIDIA/AMD GPU - Easiest Method](https://www.youtube.com/watch?v=xArcL6WVmwI)
2. [Fix Grub In Arch Linux | UEFI](https://www.youtube.com/watch?v=dF5qzXQGCR8)
3. [How to Remove Ubuntu(Linux) From Dual Boot In Windows 11/10](https://www.youtube.com/watch?v=mQyxtWrUNlE)

# Arch + Omarchy installation steps (so I don't forget)

1. Download Arch Linux [Arch Linux ISO](https://mirrors.atlas.net.co/archlinux/iso/2025.08.01/archlinux-2025.08.01-x86_64.iso).

2. If installing Arch shrinking a disk, Disk Management (in Windows) can be used. Shrink the disk and leave it
   unallocated.

3. Create a bootable USB with [BalenaEtcher](https://etcher.balena.io/) and boot into Arch installer.

4. In Arch installer:

- List disks and identify the main disk which was shrunk:

```
lsblk
```

- Use the following tool to see the **free space** available:

```
cfdisk
```

- On the free space, select "[new]" and

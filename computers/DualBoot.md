# Install from scratch a multi boot PC

_WARNING_: Self notes and Work In Progress... Only appply if you know what you are doing :).

## Preparation

- Download Ubuntu LTS DVD iso, put it on an usb stick
- Download Rocky Linux DVD iso, put it on an usb stick
- if your computer is **not** new, insure your data is correctly backup-ed
- You probably want to have another computer that works and that is bound to the internet while installing your new machine

## Hard drive setup

We install both OS on LVM. Hence, te partitions must be set up before launching the install.

- Boot your computer on the Ubuntu live system (select `Try before install` in the first list)
- Launch GParted
- Get rid of all existing partition if any, apply
- make following new partitions
  - sda1: EFI, 512 MB
  - sda2 & sda3: ext4, 2048 MB ubuntu and centOS boot
  - sda4: lvmpv, all that remains, physical volume for LVM
- apply

In a command line, launch `sudo lvm`

```sh
# create volume group
vgcreate vg_peavey /dev/sda4

# ==> TODO Check on next installation:
# - Rather only use one swap area
# - *DO NOT* create Logical Volume (root, swap...) at this point, rather create them during install of the various OSs otherwise, you might end up with duplicated volumes
# - Also try to avoid the dash '-' in LV names: they tend to be transformed in '--' in some technical locations.
# You might also create logical volume for various OS by repeating this:
# root: 20GB, swap: 8GB, but this has proven useless: the various installer create new LV volumes upon install
lvcreate -L 20GB -n ubuntu_root vg_peavey
lvcreate -L 8GB -n ubuntu_swap vg_peavey
...
```

You are now ready to install the systems.

## Installing the OS

- First install CentOs, choose sda1 as /boot/efi
- Then install Ubuntu, also choose sda1 as /boot/efi, but without formating it

## Variant: keep Windows partition

### In Windows

- Configure your windows, perform updates, etc.
- Shrink main partition with the below steps:
  - Run Disk Management: Open Run Command (`Windows button + R`) a dialog box will open and type `diskmgmt.msc`.
  - In the Disk Management screen, right-click on the partition that you want to shrink
  - select `Shrink Volume`.
- reboot

If you have multiple partitions on your hard drive, you could also choose to resize a different partition to free up space.

## Tips

### Repair USB stick

We are using Ubuntu for this tutorial, but everything applies to most modern Linux distributions. Here is how you can repair a corrupted USB drive in Linux.

#### Fix Corrupted Filesystem with FSCK

You can turn to fsck. This tool is great for removing bad file blocks, as most (if not all) corruption and unreadability comes from problems like this.

For this command, you’ll have to define the partition instead of the full drive. You’ll find it with a similar name as your device by issuing:

```sh
ls /dev/disk/by-id/usb*
```

Then, run fsck on it with:

```sh
sudo fsck -v -a /dev/disk/by-id/YOUR_FLASH_DRIVE-PARTITION-TO-CHECK
```

- `sudo fsck`: runs with administrative rights.
- `-v`: verbose flag.
- `-a`: automatically tries to repair any found error.
- `/dev/disk...` is the partition that is checked for errors.

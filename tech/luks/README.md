# Luks - Linux Unified Key Setup

## Aka. "encrypted disks on Linux"

- [https://mutschler.eu/linux/install-guides/pop-os-btrfs-21-04/](https://mutschler.eu/linux/install-guides/pop-os-btrfs-21-04/)

## Resize encrypted volume

Boot into a live-cd that matches the distro.

Then get the luks decrypted (the Disks program does this just fine).

Then resize the filesystem volume.

First find the right volume:
```
ls -la /dev/mapper/
```
(Check ID's to be sure)

Then resize:
```
e2fsck -f /dev/mapper/data-root
resize2fs -p /dev/mapper/data-root 500g # <-- new desired size in Gb
```

This takes a while...

Just to be sure, check the filesystem again:
```
e2fsck -f /dev/mapper/data-root
```

Display LVM Logical Volumes with lvdisplay.
```
lvdisplay
```
Make note of the `LV Path` needed below

To extends the logical volume to the max size:
```
lvextend -l+100%FREE /dev/path/to/your/logical/volume
```

To reduce the logical volume:
```
lvreduce -L 501G /dev/path/to/your/logical/volume
```
(This warns that it may destroy the filesystem. So take care to not redocue to much. In this exammple the new size + 1 G)

Next steps can be done with commands, but GParted makes it super easy.

Once the lvreduce has been run, GParted is able to resize the partation (both luks and physical LVM).

## Add disk that is decrypted on boot

The disk needs to be mounted. You probably did this while preparing the disk.

First of all add an entry to `/etc/fstab`:
```
UUID=<id of the EXT4 inside the Luks partition> <mount point> ext4 <options, copy from root drive> 0 0
```

So where do you get the UUID from. Well the disk is typically mounted in `/dev/mapper` so:
```
ls -la /dev/mapper/
```

It contains a bunch of `<some name> -> ../dm-<number>`. Chose the one for your new disk.

Then run this command to retrieve the UUID:
```
blkid -s UUID -o value /dev/mapper/<some name from above>
```

Second an entry should be added to `/etc/crypttab` in order for the disk to be decrypted on boot:
```
<name of your choice> UUID=<id of the Luks partiton> none luks
```

Now to retrieve the UUID of the Luks partition run:
```
blkid -s UUID -o value /dev/<what ever device your disk is>
```

Now reboot, and watch how the disk gets auto-mounted.

## Make an encrypted file volume
First create a file for the volume:
```
fallocate -l 119G vault.img
```
Then make it luks format:
```
cryptsetup --verify-passphrase luksFormat vault.img
```
Open the file volume:
```
cryptsetup open --type luks vault.img vault
```
Check that the volume is in `/dev/mapper`:
```
ls /dev/mapper
control  vault
```
Format the volume:
```
mkfs.ext4 -L vault /dev/mapper/vault
```
Mount the volume:
```
mount /dev/mapper/valut mnt/
```
To close and secure the volume, first umount:
```
umount mnt/
```
Then close the luks:
```
cryptsetup close vault
```
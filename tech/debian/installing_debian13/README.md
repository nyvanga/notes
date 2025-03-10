# Debian 13 trixie

Installing using a bootable USB stick based on the `Install DVD` the easiest way is to erase the entire drive.

This is done by choosing: `Guided - use entire disk and setup encrypted LVM`.

There is no option to do `encrypted drive without LVM`, but it might be because of how Debian boots with Grub.

To make sure everything is decrypted, it is easiest to only have 1 physical volumes. And that is achieved by having everything in an encrypted LVM:
- Loads Grub from EFI partition
- Grub loads from `/boot` partition.
- The main encrypted LUKS partition is unlocked, which is the LVM `Physical volume`.
- Root is mounted from a `Logical volume` in the `Volume group`.
- And so is the swap volume.

If installing from `Live CD` the installer complains that the `/boot` partition should also be encrypted, but the `Install DVD` doesn't seem to care about that.

## Installing Debian on disk that cannot be entirely erased

If for some reason the entire disk cannot be erased, we have to build the entire setup of partitions, `LUKS`, `LVM pvs`, `LVM vgs` and `LVM lvs` manually.

The most common reason is that the EFI partition needs to be preserved.

Fortunately that is actually possible in the installer on the `Install DVD`, it just requires a few steps in a very particular order.

### 1. EFI partition

- If there are other OS'es you want to keep, DO NOT TOUCH the EFI partition.
- Just make sure it is set to `Use as: EFI System Partition`.
- Otherwise create a 1 GB EFI System Partition at the beginning of the area.

### 2. `/boot` partition

- Create a 1 GB partition at the beginning of the free space.
- Ensure that: `Use as: Ext4 journaling file system`.
- Choose `Mount point: /boot`.
- Choose `Done setting up the partition`.

### 4. encrypted partition

- Create a partition using the rest of the free space, or as much as you feel like.
- Change to `Use as: physical volume for encryption`.
- Switch `Erase data: no` otherwise the setup will take forever erasing all data.
- `Done setting up the partition`.

### 5. encrypted `volume`

Yes an encrypted partition is not enough.

- Go to `Configure encrypted volumes`.
- Choose `Yes` to write changes to disks.
- Choose `Create encrypted volumes`.
- Chose the encrypted partition from above. IMPORTANT to choose the right one.
- Choose `Continue`.
- Choose `Finish`.
- It will now prompt for the password.

### 6. Setup LVM

- Go to `Configure the Logical Volume Manager`.
- It is easier to have root and swap partition in on encrypted volume when booting. So we use LVM to achieve that.
- Choose `Yes` to write changes to disks, even though there shouldn't be any.
- Choose `Create volume group`.
- Give it a name. Standard is `<hostname>-vg`.
- Select the encrypted device for this volume group.
- Typically named `/dev/mapper/<some device name>_crypt`.
- Choose `Continue`.
- Now choose `Create logical volume` for the root filesystem.
  . Chose the `Volume group` from above.
  - Give it a name. Standard is `<hostname>-<volume group name>-root`.
  - Set the size.
    - It should be sized to whatever the free space is, minus what you intend to use for swap.
- Choose `Create logical volume` for swap. Same as above, but use the rest of the free space.
- Choose `Finish`.

### 7. `/` (root) volume

Back in the partitioner the now logical volumes should show up.

- Find the `Logical volume` for root.
- Change to `Use as: Ext4 journaling file system`.
- And `Mount point: /` (root).
- `Done setting up the partition`.

### 8. `swap` volume

- Find the `Logical volume` for swap.
- Change to `Use as: swap area`.
- `Done setting up the partition`.

### 9. All done

There should now be volumes for `/` (root), `/boot` and `swap`.

So cross your fingers and choose:
- `Finish partitioning and write changes to disk`.

A summary is displayed. Check it and choose `Yes` to write changes to disk and continue.

The installer will complain (loudly) and not let you continue if anything is missing.

Now the install should continue as normal. And Grub should be configure to boot and prompt for the encryption password for the disk.

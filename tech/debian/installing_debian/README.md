# Installing Debian

Get a DVD image and write to an USB stick.

Use [Rufus](https://rufus.ie/) to write the USB stick.

If you wan't a UEFI bootable Debian linux, make sure:
1. Write the USB stick as UEFI bootable
1. Boot it with UEFI.

There is NO WAY to change this once installed.

Network is needed during install, and some NIC's (particular Wifi) needs special nonfree firmwares.

Either [download the firmware.zip](https://cdimage.debian.org/cdimage/unofficial/non-free/firmware/stable/current/) and write to USB, or choose an image with firmware included.

## Current or stable DVD images (torrents)
- [Install DVD](https://cdimage.debian.org/cdimage/release/current/amd64/bt-dvd/)
- [Live DVD](https://cdimage.debian.org/cdimage/release/current-live/amd64/bt-hybrid/)
- [Install DVD with firmware](https://cdimage.debian.org/cdimage/unofficial/non-free/images-including-firmware/current/amd64/bt-dvd/)
- [Live DVD with firmware](https://cdimage.debian.org/cdimage/unofficial/non-free/images-including-firmware/current-live/amd64/bt-hybrid/)

## Testing (weekly-builds) DVD images
- [Install DVD](https://cdimage.debian.org/cdimage/weekly-builds/amd64/iso-dvd/)
- [Live DVD](https://cdimage.debian.org/cdimage/weekly-live-builds/amd64/iso-hybrid/)
- [Install DVD with firmware](https://cdimage.debian.org/cdimage/unofficial/non-free/images-including-firmware/weekly-builds/amd64/iso-dvd/)
- [Live DVD with firmware](https://cdimage.debian.org/cdimage/unofficial/non-free/images-including-firmware/weekly-live-builds/amd64/iso-hybrid/)

## Debian 11 bullseye

Missing `libappindicator3-1` problems. Well it is deprecated, so it should be missing, but is installable from here:
- [libappindicator3-1](https://packages.debian.org/buster/libappindicator3-1)
   - [libindicator3-7](https://packages.debian.org/buster/libindicator3-7)

Missing `libappindicator1` same but different:
- [libappindicator1](https://packages.debian.org/buster/libappindicator1)
   - [libindicator7](https://packages.debian.org/buster/libindicator7)
   - [libdbusmenu-gtk4](https://packages.debian.org/buster/libdbusmenu-gtk4)

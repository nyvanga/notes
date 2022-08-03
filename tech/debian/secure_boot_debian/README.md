# Secure boot Debian

I has been supported since buster according to [What is UEFI Secure Boot?](https://wiki.debian.org/SecureBoot#What_is_UEFI_Secure_Boot.3F) documentation.

## Check secure boot status

Easy:
```bash
sudo mokutil --sb-state
```

## BIOS settigns

First enable secure boot (if disabled) in your BIOS settings.

On my laptop there was a `Deployed mode` and `Audit mode`. Audit mode lets you boot even though secure boot is not yet configured correctly. This is the setting you want while troubleshooting this.

## Disabling/re-enabling Secure Boot

First I just tried to enable secure boot, but in the `MOK` screens it returned error `Could not delete secure boot state`.

So I then tried disabling secure boot:
```bash
sudo mokutil --disable-validation
```
([Disabling/re-enabling Secure Boot](https://wiki.debian.org/SecureBoot#Disabling.2Fre-enabling_Secure_Boot))


And then enabling secure boot:
```bash
sudo mokutil --enable-validation
```
Which worked without errors.

After switching from `Audit mode` to `Deployed mode` the status check above returned:
```bash
SecureBoot enabled
```

BUT! My Nvidia DKMS drivers were not loaded, problably since they are not signed correctly.

## DKMS and Secure Boot

So to make the Nvidia DKMS drivers load again, I re-generated the MOK keys and enrolled them.

There was already keys in `/var/libs/dkms` but I ran this command anyway:
```bash
sudo dkms generate_mok
```

Then enrolled the keys:
```bash
sudo mokutil --import /var/lib/dkms/mok.pub
```
This will prompt for a one-time password, to be used at the next boot.

Then checked that the keys would be prompted at next boot:
```bash
sudo mokutil --list-new
```

And rebooted. On the mok screen chose `Enroll MOK` type the one-time password from above and follwed instructions, and then chose `Reboot`.

Now the Nvidia drivers loaded successfully and worked.

([DKMS and Secure Boot](https://wiki.debian.org/SecureBoot#DKMS_and_Secure_Boot))

## Lenovo laptop BIOS

For some reason I couldn't make secure boot work on my Lenovo Thinkpad P14s.

Then after messing with all settings and mokutil commands I noticed an option way at the bottom of the settings called `Allow Microsoft 3rd Party UEFI CA`.

And after thinking about the mention of [Microsoft signing the Debian UEFI shim](https://wiki.debian.org/SecureBoot#Shim) in the documentation I enabled it. Now everything works!

# Windows won't boot...

So you have f'd up your Windows boot (EFI) partition or something.

Don't panic! It can usually be rescued.

## So you re-installed Linux... and lost the Windows part of EFI

Well boot a Windows USB stick, and go into `Command Prompt`

Setup drive letters:
```
diskpart
list disk
select disk #
list partition
select partition #   # Choose EFI partition, typically 300-500MB
assign letter=n:
select partition #   # Choose Windows partition
assign letter=c:
exit
```

Fix boot:
```
c:
bcdboot c:\windows /s n: /f all
bootrec /scanos  #   # Make sure you see it detect C:\Windows or similar
bootrec /fixboot #   # Ignore if you get access denied.
bootrec /rebuildbcd
```

If it worked, Windows will boot by default.

Now enter your bios and change boot to `your Linux distro` instead of `Windows Bootmanager`.

Once booted into Linux run `sudo update-grub` to get the rediscovered Windows into your Grub boot menu.

## EFI partition deleted or otherwise gone?

- [How to Restore Deleted EFI Boot Partition in Windows 10](http://woshub.com/how-to-repair-deleted-efi-partition-in-windows-7/)

Create/recreate EFI and MSR partition:
```
DiskPart
list disk
select disk <number of the disk shown by list>
list partition
select partition <number of EFI partition>
delete partition override
list partition
select partition <number of MSR partition>
delete partition override
create partition efi size=600
create partition msr size=16
list partition
select partition <number of EFI partition>
format quick fs=fat32 label="System"
assign letter=G
list vol
select vol <number of Windows volume>
assign letter=C
exit
```

Update: Instead of doing all this manually below, it seems `bootrec /rebuildbcd` more or less does the same. So maybe try that first.

Setup EFI to boot windows (asumes EFI mounted on G: and Windows on C:):
```
mkdir G:\EFI\Microsoft\Boot
xcopy /s C:\Windows\Boot\EFI\*.* G:\EFI\Microsoft\Boot
g:
cd EFI\Microsoft\Boot
bcdedit /createstore BCD
bcdedit /store BCD /create {bootmgr} /d “Windows Boot Manager”
bcdedit /store BCD /create /d “My Windows 10” /application osloader
```
The above command produces a GUID that should be used below in `{your_guid}`.
```
bcdedit /store BCD /set {bootmgr} default {your_guid}
bcdedit /store BCD /set {bootmgr} path \EFI\Microsoft\Boot\bootmgfw.efi
bcdedit /store BCD /set {bootmgr} displayorder {default}
bcdedit /store BCD /set {default} device partition=c:
bcdedit /store BCD /set {default} osdevice partition=c:
bcdedit /store BCD /set {default} path \Windows\System32\winload.efi
bcdedit /store BCD /set {default} systemroot \Windows
exit
```


## Installed Windows on a non GPT HD with legacy boot?

- [How to Change Legacy to UEFI Without Reinstalling Windows 10](https://www.wintips.org/how-to-change-legacy-to-uefi-without-reinstall-windows-10/)

## Windows Recovery partition

- [Get Comprehensive Understanding of Windows 10 Recovery Partition](https://www.partitionwizard.com/partitionmagic/windows-10-recovery-partition.html)
- [Can I Delete Recovery Partition in Windows 7/8/10 for Further Use](https://www.partitionwizard.com/partitionmagic/delete-recovery-partition.html)

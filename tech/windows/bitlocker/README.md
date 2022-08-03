# Bitlocker

[How to Use BitLocker Without a Trusted Platform Module (TPM)](https://www.howtogeek.com/6229/how-to-use-bitlocker-on-drives-without-tpm/)

**TL;DR;**
1. Open the Local Group Policy Editor (Run `gpedit.msc`)
1. In left pane navigate to:
   1. `Computer Policy`
   1. `Computer Configuration`
   1. `Administrative Templates`
   1. `Windows Components`
   1. `BitLocker Drive Encryption`
   1. `Operating System Drives`
1. Edit `Require additional authentication at startup`
1. Switch to `Enabled`
1. Ensure `Allow BitLocker without a compatible TPM (requires a password or a startup key on a USB flash drive)` is enabled.
1. Clock `Ok`.


## Switching BitLocker protection methods without re-encrypting

[Source. Not quite up to date.](https://www.stevenmaude.co.uk/posts/switching-bitlocker-protection-methods-without-re-encrypting)

In an admin prompt, to check "protectors" status:
```
manage-bde -protectors -get <drive>

```

Remove TMP (leaves recovery key intact):
```
manage-bde -protectors -delete <drive> -type TPM

```

Add password protector:
```
manage-bde -protectors -add <drive> -password

```
It might complain that:
```
ERROR: An error occurred (code 0x8031006a):
Group Policy settings do not permit the creation of a password.
```
If that is the case, follow the howto geek above to enable that policy.

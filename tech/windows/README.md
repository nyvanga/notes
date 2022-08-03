# Windows

- [Windows won't boot...](noboot/)

- [Windows Update shortcut on the Start menu](windows_update_shortcut/)

- [WSL 2](wsl2/)

- [Docker](docker/)

- [Hyper-V](hyperv/)


# Windows update trouble

- [Windows Updates: Package failed to be changed to the Installed state. Status: 0x800f0922](https://answers.microsoft.com/en-us/windows/forum/all/windows-updates-package-failed-to-be-changed-to/4154ea0a-8426-421c-85cc-e30b54230fec)

# Windows "Fast Startup" crap

So Windows doesn't shut down, when you shut down. It uses some kind of franken-hibernate solution, to make the next power-on boot faster.

A restart on the other hand actually completely shuts down the computer, before booting againg.

(Some kind of MS logic i guess...)

Anyway, if you have problems with you're computer when it is first powered on, and an immediate restart fixes the issue, chances are fast startup is the problem.

Disable it by:
1. Going to "Power Options" (chose you're favorite route to it...)
1. Select "Choose what the power buttons do"
1. On this screen you see "Turn on fast startup (recommended)" (I don't recommended it!!!) under "Shutdown settings"
1. Click "Change settings that are currently unavailable" further up the screen, to enable the checkboxes
1. And disable that sucker!

# Windows update broken

Some times (especially when a rollback to a restore point) Windows Update gets into a funky state.
To reset Windows update, and force it to rediscover which updates are actually needed, run:

```
net stop bits

net stop wuauserv

ren %systemroot%\softwaredistribution softwaredistribution.bak

ren %systemroot%\system32\catroot2 catroot2.bak

net start bits

net start wuauserv

```
[Fejlfinding af problemer under opdatering af Windows](https://support.microsoft.com/da-dk/windows/fejlfinding-af-problemer-under-opdatering-af-windows-188c2b0f-10a7-d72f-65b8-32d177eb136c)

# Bitlocker

[How to Use BitLocker Without a Trusted Platform Module (TPM)](https://www.howtogeek.com/howto/6229/how-to-use-bitlocker-on-drives-without-tpm/)

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

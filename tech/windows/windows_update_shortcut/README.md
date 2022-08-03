# Windows update on the start menu

In Windows 11 you cannot add the "Windows Update" icon to the start menu. Thx Micro$soft!!!

Obviously there is a work-around...

First open the Start menu folder:
```
%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs
```

Then right click and choose `New` --> `Shortcut`

In the `Type the location of the item:` insert:
```
%windir%\explorer.exe ms-settings:windowsupdate-action
```

The `Type a name for this shortcut:` will be prefilled with `explorer.exe` so choose something better:
```
Windows Update
```

Now the icon will be the icon of `explorer.exe` so to change:
1. Right-click and choose `Properties`
1. Click `Change Icon...`
1. Choose a better icon.

```
C:\Windows\System32\Shell32.dll
```

The new shortcut should show up automatically on the Start menu under pinned.

If not go find it in `All apps` and pin it to Start menu and/or the taskbar.
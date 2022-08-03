# flatpak

Flatpak is not great. But it is soo much better and less annoying than snaps.

So install your favourite distro (which is Debian of course), and then add flatpak for those last programs you cannot find as deb's.

```
sudo apt install flatpak gnome-software-plugin-flatpak
```

```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

[Source](https://flatpak.org/setup/Debian)

## When updating flatpak's over SSH

When connecting over SSH to a machine with flatpak, and updating with:
```
flatpak update
flatpak uninstall --unused
```

You might get an error message:
```
==== AUTHENTICATING FOR org.freedesktop.Flatpak.modify-repo ====
```
Or
```
==== AUTHENTICATING FOR org.freedesktop.Flatpak.appstream-update ====
```
And then get asked for your password repeatedly.

There are a few discussion online on [what to do](https://github.com/flatpak/flatpak/issues/3001#issuecomment-2098981907).

tl;dr

Add a file `/etc/polkit-1/rules.d/Flatpak-ssh-override.rules` with:
```
polkit.addRule(function(action, subject) {
    if ((action.id == "org.freedesktop.Flatpak.app-install" ||
         action.id == "org.freedesktop.Flatpak.runtime-install"||
         action.id == "org.freedesktop.Flatpak.app-uninstall" ||
         action.id == "org.freedesktop.Flatpak.runtime-uninstall" ||
         action.id == "org.freedesktop.Flatpak.modify-repo" ||
         action.id == "org.freedesktop.Flatpak.appstream-update") &&
        subject.active == true &&
        subject.isInGroup("sudo")) {
            return polkit.Result.YES;
    }

    return polkit.Result.NOT_HANDLED;
});
```
Check in `/usr/share/polkit-1/rules.d/org.freedesktop.Flatpak.rules`, the above is the first `polkit.addRule` but with `subject.local == true` removed.

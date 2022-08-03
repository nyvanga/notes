# Firefox alternatives

Well don't use plain Firefox since:
- [promises to not sell their data go up in smoke](https://www.theregister.com/2025/03/02/mozilla_introduces_terms_of_use/)
- [Mozilla announced that AI would be its top priority in 2025](https://blog.mozilla.org/netpolicy/2025/03/27/mozilla-shares-2025-policy-priorities-and-recommendations-for-creating-an-internet-where-everyone-can-thrive/)

For desktop use [LibreWolf](https://librewolf.net/installation/debian/).

Also available for [Windows](https://librewolf.net/installation/windows/), but if your are using Windows, you already accepted the Micro$oft [enshittification](https://en.wikipedia.org/wiki/Enshittification), so you might as well just use plain Firefox.

On Android use [IronFox](../android/get_apps/).

# Popups

Load Web browser links into a (new window=2, new tab=3, current tab=1)
```
browser.link.open_newwindow = 3
```
Load external links into a (new window=2, new tab=3, current tab=1)
```
browser.link.open_newwindow.override.external = 3
```
Load these popups into existing windows (all popups=0, unresized popups=2, none=1)
```
browser.link.open_newwindow.restriction = 0
```

# Domain Guessing

```
browser.fixup.alternate.enabled = false
```

# Search from address bar

```
keyword.enabled = false
browser.urlbar.oneOffSearches = false
```

# Translate popup

```
browser.translations.automaticallyPopup = false
```

# Media autoplay

Some settings are not available in the UI:
```
media.autoplay.default = 5
media.autoplay.blocking_policy = 2
media.autoplay.allow-extension-background-pages = false
media.autoplay.block-event.enabled = true
```

# Keyboard shortcuts

Some websites likes to redefine keyboard shortcuts in the browser (unknown=0, allow=1, block=2, prompt=3):
```
permissions.default.shortcuts = 2
```
Prompt doesn't seem to work.

# Plugins

- [Ublock origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)
   - [Import 3rd-party filter lists](https://github.com/gorhill/uBlock/wiki/Filter-lists-from-around-the-web#import-3rd-party-filter-lists)
- [Privcay badger](https://addons.mozilla.org/en-US/firefox/addon/privacy-badger17/)
- [Bitwarden](https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager/)
- [Video DownloadHelper](https://addons.mozilla.org/en-US/firefox/addon/video-downloadhelper/)

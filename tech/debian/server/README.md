# Debian on a server

## Disable suspend and hibernate completely

```
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

[Source](https://wiki.debian.org/Suspend#Disable_suspend_and_hibernation)

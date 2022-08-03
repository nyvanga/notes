# printk

[printk](https://en.wikipedia.org/wiki/Printk) on wikipedia.

For years I was really annoyed, that the kernel logs were printed to the console of a linux server.

Logging firewall drops sometimes completely swamped the console, making it difficult to type.

Turns out the logging to console can be turned down really easily, by configuring `printk`.

## Configure `printk` to be less annoying

So first of check the current `printk` settings:
```
cat /proc/sys/kernel/printk
```

It will output something like:
```
4	4	1	7
```
Which translates to:

|                   | console_loglevel | default_message_loglevel | minimum_console_loglevel | default_console_loglevel |
|-------------------|------------------|--------------------------|--------------------------|--------------------------|
| 0 - emergency     | x                |                          |                          | x                        |
| 1 - alert         | x                |                          | x                        | x                        |
| 2 - critical      | x                |                          |                          | x                        |
| 3 - error         | x                |                          |                          | x                        |
| 4 - warning       | x                | x                        |                          | x                        |
| 5 - notice        |                  |                          |                          | x                        |
| 6 - informational |                  |                          |                          | x                        |
| 7 - debug         |                  |                          |                          | x                        |

From: [Runtime configuring printk – stop kernel printk from flooding the console](https://sudousr.wordpress.com/2015/11/26/runtime-configuring-printk-stop-kernel-printk-to-flooding-console/)

And the default console loglevel of 7 is way too loud. And normally the advice online is to set it to 3.

Or rather to set it to `7	4	1	3`.

This most commonly done by editing `/etc/sysctl.conf` and uncomment the following:
```
# Uncomment the following to stop low-level messages on console
# kernel.printk = 3 4 1 3
```

Some distros (Debian Trixie) doesn't include a `/etc/sysctl.conf`, but then add a file like `/etc/sysctl.d/my.conf` with the line `kernel.printk = 3 4 1 3`.

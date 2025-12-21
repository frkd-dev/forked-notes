---
date: 2021-03-01
share: true
tags:
  - linux
  - systemd
---
_The Resilio Sync documentation is giving a wrong advice for Linux users on how to handle its configuration files. Here is a better way._

In the default setup, Resilio Sync on Debian-based distros (Ubuntu, Mint, etc.) is running as "rslsync" user which in some scenarios is inconvenient. The official guide has a solution on how to run it as a regular user but it consists of one questionable part:


> If you want to run Sync under your current user — edit file /usr/lib/systemd/user/resilio-sync.service and change WantedBy=multi-user.target to WantedBy=default.target.

— _Resilio official documentation_

Editing system files which are belonging to the installed package is the wrong way to manage stuff in Linux. If you'll follow this guide then you might lost your edits once the package gets an update and this will break up all things.

The proper way for the Debian (and other systemd based distros) would be to use [systemd drop-ins](https://www.freedesktop.org/software/systemd/man/systemd.unit.html) functionality to modify the service's behavior without direct editing files belonging to the installed package. Drop-ins are separate configuration files that could overlay the systemd unit settings by "shadowing" certain parameters.

Here is a final set of commands to run Resilio Sync as a regular user:

```sh
# Disable default service used to run as `rslsync` user
sudo systemctl stop resilio-sync.service
sudo systemctl disable resilio-sync.service

# Create a drop-in. Notice omitted sudo below this point.
systemctl --user edit resilio-sync.service

# At this point you'll get an editor opened where you need to
# add two following lines which will replace in initial unit file:
# [Install]
# WantedBy=default.target

# Now enable and start service as the current user.
systemctl --user enable resilio-sync
systemctl --user start resilio-sync
```
From that moment all further actions toward the Resilio Sync service should be issued with `systemctl --user` and without sudo. Configuration files for such service instances can be found in the `~/.config/resilio-sync` folder.
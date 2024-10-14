# Counter-Strike 1.6 Dedicated Server for Linux

![logo](images/logo.jpg)

## Requirements

Make sure you have 32-bit libraries before installation:

- `pacman -S lib32-gcc-libs` for Arch (enable multilib repo first)
- `apt install lib32stdc++6` for Debian based distros
- `dnf install libstdc++.i686` for RHEL based distros

And basic tools:

- `tar xz gzip curl`

## Installation

Run `./install` script as root, first it downloads these files to `/tmp/cs16-server`:

- **steamcmd**
- **metamod-p-v1.21p38**
- **amxmodx-1.10-latest**
- **yapb-4.4.957**

Once it's done it places `cs16-server` script that controls the server to `/usr/bin` and `cs16-server.conf` with all hlds args to `/etc/hlds`.

It also creates `hlds` user that extracts all acrhives from `/tmp/cs16-server` to `/home/hlds/.steam` directory, makes necessary symlinks and updates **steamcmd**.

`hlds` user is created with account locked, you need to use `sudo su - hlds` if you plan to do further server configuration in `/home/hlds/.steam` directory.

yapb bots are disabled by default, if you want to enable them uncomment `;;linux addons/yapb/bin/yapb.so` line in `cs16/cstrike/addons/metamod/plugins.ini`.

## Usage

`Usage: cs16-server [start | restart | stop | update | status]`

## cs16-server.conf

```
server_params="-pingboost 3 -maxplayers 32 +sv_lan 0 +map de_dust2"
server_dll="-dll cstrike/addons/metamod/dlls/metamod.so"
server_game="-game cstrike -secure"
```

`server_params` can be changed to whatever you want to, it all just passes options to `hlds_run`.

Leave `server_dll` var empty to disable metamod and have a pure vanilla server without any mods.

## SELinux

To make the systemd service work with SELinux you need to enter the following:

```
semanage fcontext -a -t bin_t /home/hlds/.steam/cs16/hlds_run
restorecon -v /home/hlds/.steam/cs16/hlds_run
```

Alternatively SELinux can be disabled entirely by setting `SELINUX=disabled` in `/etc/selinux/config` and entering `setenforce 0`.

## Removal

Run `./install remove` to uninstall the server and its files along with `hlds` user.

## Extra

A big map pack is available on the [Internet Archive](https://archive.org/details/cs-1.6-mega-map-pack-v-2018.1.7z).

Bot waypoints for maps are downloaded automatically if there's no `.graph` file for a map, but you can also get them all from [yapb/graph](https://github.com/yapb/graph) repo.

- [metamod-p](https://github.com/Bots-United/metamod-p)
- [amxmodx](https://github.com/alliedmodders/amxmodx)
- [yapb](https://github.com/yapb/yapb)

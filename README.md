# Counter-Strike 1.6 Dedicated Server for Linux

![logo](images/logo.jpg)

## Requirements

Make sure you have the required 32-bit libraries before installation:

- `apt install lib32stdc++6` for Debian-based distros
- `dnf install libstdc++.i686` for RHEL-based distros
- `pacman -S lib32-gcc-libs` for Arch

## Installation

- `./install`

First, it downloads these files to `/tmp/hlds`:

- [steamcmd](https://developer.valvesoftware.com/wiki/SteamCMD)
- [metamod-p-v1.21p109](https://github.com/Bots-United/metamod-p)
- [amxmodx-1.10.0.5476](https://github.com/alliedmodders/amxmodx)
- [yapb-4.4.957](https://github.com/yapb/yapb)

Once it's done, it places the `cs16-server` script that controls the server into `/usr/bin` and the `cs16-server.conf` with all `hlds_args` into `/etc/hlds`.

It also creates the `hlds` user, which extracts all archives from `/tmp/hlds` to the `/opt/hlds/.steam` directory, makes necessary symlinks, and updates **steamcmd**.

> [!NOTE]
> - The `hlds` user is created with the account locked. You need to use `sudo -u hlds bash` if you plan to do further server configuration in the `/opt/hlds/cs16` directory.
> - To get AMX Mod X admin access, edit `cstrike/addons/amxmodx/configs/users.ini` accordingly.
> - YaPB bots are disabled by default. If you want to enable them, uncomment the line with `yapb.so` in `cstrike/addons/metamod/plugins.ini`.

## Usage

`cs16-server <start | stop | restart | update | status>`

## cs16-server.conf

`hlds_args="-secure -pingboost 3 -maxplayers 32 +sys_ticrate 1024 +sv_lan 0 +map de_dust2 -dll addons/metamod/dlls/metamod.so"`

## Removal

- `./install remove` to uninstall the server and its files along with the `hlds` user.

## Extra

A big map pack is available on the [Internet Archive](https://archive.org/details/cs-1.6-mega-map-pack-v-2018.1.7z).

Bot waypoints for maps are downloaded automatically if there's no `.graph` file for a map, but you can also get them all from [yapb/graph](https://github.com/yapb/graph) repo.

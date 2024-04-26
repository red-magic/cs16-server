# Counter-Strike 1.6 Dedicated Server for Linux

![logo](images/logo.jpg)

## Requirements

Make sure you have 32 bit libraries before installation:

- `pacman -S lib32-gcc-libs` for Arch (enable multilib repo first)
- `apt install lib32stdc++6` for Debian based distros
- `dnf install glibc.i686 libstdc++.i686` for RHEL based distros

And basic tools: `sudo tar xz gzip curl`

## Installation

Run `./install` script, first it downloads **steamcmd**, **metamod-p-v1.21p38**, **amxmodx-1.10-latest** and **yapb-4.4.957** to `/tmp/cs16-server`.

Once it's done it places `cs16-server` main script to `/usr/bin` and `cs16-server.conf` with `$server_params` to `/etc/hlds`.

It also creates `hlds` user which launches `install-stage-two` script.

`install-stage-two` is run by `hlds` user and basically extracts all acrhives from `/tmp/cs16-server` to `/home/hlds/.steam` directory, makes necessary symlinks and updates **steamcmd**.

`hlds` user is created with account locked and password disabled, you need to use `sudo su - hlds` if you want to do further server configuration in `/home/hlds/.steam` directory.

yapb bots are disabled by default, if you want to enable them uncomment `;;linux addons/yapb/bin/yapb.so` line in `cs16/cstrike/addons/metamod/plugins.ini`.

## Usage

`Usage: cs16-server [start | restart | stop | update | status]`

## cs16-server.conf

```
server_params="-pingboost 3 -maxplayers 32 +sv_lan 0 +map de_dust2"
server_dll="-dll cstrike/addons/metamod/dlls/metamod.so"
server_game="-game cstrike -secure"
```

You can change `server_params` to whatever you want to, it all just passes options to `hlds_run`.
Comment out `server_dll` line with `#` if you want to disable metamod and have a pure vanilla server without any mods.

## Removal

Run `./install remove` to uninstall and clean everything.

User `hlds` will be deleted. The following directories and files will also be deleted:

```
/home/hlds
/etc/hlds
/usr/bin/cs16-server
```

## Extra

A big map pack is available on the [Internet Archive](https://archive.org/details/cs-1.6-mega-map-pack-v-2018.1.7z).

Bot waypoints for maps are downloaded automatically if there's no `.graph` file for a map, but you can also get them all from [yapb/graph](https://github.com/yapb/graph) repo.

- [metamod-p](https://github.com/Bots-United/metamod-p)
- [amxmodx](https://github.com/alliedmodders/amxmodx)
- [yapb](https://github.com/yapb/yapb)

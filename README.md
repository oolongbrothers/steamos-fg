# steamos-fg

## What is this for?

When using the SteamOS compositor, some games start behind the big picture UI and no graphics are displayed. This script forces such games to be shown in the foreground.

Affected games this script fixes include:

 - Dead Cells
 - Dirt Rally
 - Civilization VI
 - The Count Lucanor
 - Mad Max
 - Mirror's Edge (Steam Play)

Also games installed as snaps and flatpaks do share this fate and can be fixed by wrapping them with this script.

## Why this fork?

This is an actively maintained fork of https://github.com/alkazar/steamos-fg.

Big thanks to [alkazar](https://github.com/alkazar) for the original code.

## Installation

Run the following command:

`sudo curl https://raw.githubusercontent.com/oolongbrothers/steamos-fg/master/steamos-fg -o /usr/local/bin/steamos-fg && sudo chmod a+x /usr/local/bin/steamos-fg`

## Usage

### Steam games

Add `steamos-fg %command%` to the launch options for each game you wish to use this script with.

### Games imported from desktop shortcuts

You can do this is by editing the `.desktop` files and prepending the command with the script path.

Example locations of `.desktop` files, here for Debian/Ubuntu:

|Package format|system-wide|user|
|---:|:---|:---|
|**deb**|`/usr/share/applications/`|`${HOME}/.local/share/applications/`|
|**Snap**|`/var/lib/snapd/desktop/applications`|-|
|**Flatpak**|`/var/lib/flatpak/exports/share/applications`|`${HOME}/.local/share/flatpak/exports/share/applications`|

There is a gotcha however: In its "Add Desktop Shortcut" GUI, Steam ignores all desktop shortcuts where the executable has *steam* in the filename (is is fine to have the string *steam* appear in the arguments though).

To fix this, create a link to `steamos-fg` with a different name:

```
ln -s /usr/local/bin/steamos-fg /usr/local/bin/gameros-fg
```

Then you can change those `.desktop` files by prepending `/usr/local/bin/gameros-fg` to the value of the `Exec` property:

Snap example:

```
Exec=/usr/local/bin/gameros-fg env BAMF_DESKTOP_FILE_HINT=/var/lib/snapd/desktop/applications/xyz.desktop /snap/bin/xyz %U
```

Flatpak example:

```
Exec=/usr/local/bin/gameros-fg /usr/bin/flatpak run --branch=stable --arch=x86_64 --command=xyz xyz
```

## Notes

This script was tested on Ubuntu and but should work on SteamOS and Arch.

## Package dependencies

### Arch

On Arch, you will need these packages:

- `xorg-xwininfo`
- `xorg-xprop`
- `xdotool`
- `procps-ng`
- `gawk`

### Ubuntu

On Ubuntu, you will need these packages:

- `x11-utils`
- `xdotool`
- `procps`
- `gawk`

## Alternatives

Instead of this hack, you could fix the problems where they stem from, the `steamos-compositor`.

For a fixed version of `steamos-compositor`, have a look at [steamos-compositor-plus](https://github.com/gamer-os/steamos-compositor-plus), which is part of SteamOS alternative [GamerOS](https://gamer-os.github.io/) by [alkazar](https://github.com/alkazar), the original author of steamos-fg.

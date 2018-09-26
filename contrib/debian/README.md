
Debian
====================
This directory contains files used to package injexd/injex-qt
for Debian-based Linux systems. If you compile injexd/injex-qt yourself, there are some useful files here.

## injex: URI support ##


injex-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install injex-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your injex-qt binary to `/usr/bin`
and the `../../share/pixmaps/injex128.png` to `/usr/share/pixmaps`

injex-qt.protocol (KDE)


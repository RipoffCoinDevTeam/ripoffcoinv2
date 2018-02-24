
Debian
====================
This directory contains files used to package ripoffcoind/ripoffcoin-qt
for Debian-based Linux systems. If you compile ripoffcoind/ripoffcoin-qt yourself, there are some useful files here.

## ripoffcoin: URI support ##


ripoffcoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install ripoffcoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your ripoffcoin-qt binary to `/usr/bin`
and the `../../share/pixmaps/ripoffcoin128.png` to `/usr/share/pixmaps`

ripoffcoin-qt.protocol (KDE)


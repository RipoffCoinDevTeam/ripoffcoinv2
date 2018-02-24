Sample init scripts and service configuration for ripoffcoind
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/ripoffcoind.service:    systemd service unit configuration
    contrib/init/ripoffcoind.openrc:     OpenRC compatible SysV style init script
    contrib/init/ripoffcoind.openrcconf: OpenRC conf.d file
    contrib/init/ripoffcoind.conf:       Upstart service configuration file
    contrib/init/ripoffcoind.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three Linux startup configurations assume the existence of a "ripoffcoincore" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes ripoffcoind will be set up for the current user.

2. Configuration
---------------------------------

At a bare minimum, ripoffcoind requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, ripoffcoind will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that ripoffcoind and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If ripoffcoind is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running ripoffcoind without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/ripoffcoin.conf`.

3. Paths
---------------------------------

3a) Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/ripoffcoind`  
Configuration file:  `/etc/ripoffcoincore/ripoffcoin.conf`  
Data directory:      `/var/lib/ripoffcoind`  
PID file:            `/var/run/ripoffcoind/ripoffcoind.pid` (OpenRC and Upstart) or `/var/lib/ripoffcoind/ripoffcoind.pid` (systemd)  
Lock file:           `/var/lock/subsys/ripoffcoind` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the ripoffcoincore user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
ripoffcoincore user and group.  Access to ripoffcoin-cli and other ripoffcoind rpc clients
can then be controlled by group membership.

3b) Mac OS X

Binary:              `/usr/local/bin/ripoffcoind`  
Configuration file:  `~/Library/Application Support/ripoffcoinv2/ripoffcoin.conf`
Data directory:      `~/Library/Application Support/ripoffcoinv2`
Lock file:           `~/Library/Application Support/ripoffcoinv2/.lock`

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start ripoffcoind` and to enable for system startup run
`systemctl enable ripoffcoind`

4b) OpenRC

Rename ripoffcoind.openrc to ripoffcoind and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/ripoffcoind start` and configure it to run on startup with
`rc-update add ripoffcoind`

4c) Upstart (for Debian/Ubuntu based distributions)

Drop ripoffcoind.conf in /etc/init.  Test by running `service ripoffcoind start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

4d) CentOS

Copy ripoffcoind.init to /etc/init.d/ripoffcoind. Test by running `service ripoffcoind start`.

Using this script, you can adjust the path and flags to the ripoffcoind program by
setting the RIPOFFCOIND and FLAGS environment variables in the file
/etc/sysconfig/ripoffcoind. You can also use the DAEMONOPTS environment variable here.

4e) Mac OS X

Copy org.ripoffcoin.ripoffcoind.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.ripoffcoin.ripoffcoind.plist`.

This Launch Agent will cause ripoffcoind to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run ripoffcoind as the current user.
You will need to modify org.ripoffcoin.ripoffcoind.plist if you intend to use it as a
Launch Daemon with a dedicated ripoffcoincore user.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.

Sample init scripts and service configuration for injexd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/injexd.service:    systemd service unit configuration
    contrib/init/injexd.openrc:     OpenRC compatible SysV style init script
    contrib/init/injexd.openrcconf: OpenRC conf.d file
    contrib/init/injexd.conf:       Upstart service configuration file
    contrib/init/injexd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three Linux startup configurations assume the existence of a "injexcore" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes injexd will be set up for the current user.

2. Configuration
---------------------------------

At a bare minimum, injexd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, injexd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that injexd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If injexd is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running injexd without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/injex.conf`.

3. Paths
---------------------------------

3a) Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/injexd`  
Configuration file:  `/etc/injexcore/injex.conf`  
Data directory:      `/var/lib/injexd`  
PID file:            `/var/run/injexd/injexd.pid` (OpenRC and Upstart) or `/var/lib/injexd/injexd.pid` (systemd)  
Lock file:           `/var/lock/subsys/injexd` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the injexcore user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
injexcore user and group.  Access to injex-cli and other injexd rpc clients
can then be controlled by group membership.

3b) Mac OS X

Binary:              `/usr/local/bin/injexd`  
Configuration file:  `~/Library/Application Support/InjexCore/injex.conf`  
Data directory:      `~/Library/Application Support/InjexCore`
Lock file:           `~/Library/Application Support/InjexCore/.lock`

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start injexd` and to enable for system startup run
`systemctl enable injexd`

4b) OpenRC

Rename injexd.openrc to injexd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/injexd start` and configure it to run on startup with
`rc-update add injexd`

4c) Upstart (for Debian/Ubuntu based distributions)

Drop injexd.conf in /etc/init.  Test by running `service injexd start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

4d) CentOS

Copy injexd.init to /etc/init.d/injexd. Test by running `service injexd start`.

Using this script, you can adjust the path and flags to the injexd program by
setting the INJD and FLAGS environment variables in the file
/etc/sysconfig/injexd. You can also use the DAEMONOPTS environment variable here.

4e) Mac OS X

Copy org.injex.injexd.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.injex.injexd.plist`.

This Launch Agent will cause injexd to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run injexd as the current user.
You will need to modify org.injex.injexd.plist if you intend to use it as a
Launch Daemon with a dedicated injexcore user.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.

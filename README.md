# openrc-wrapper

Using some __openrc__ scripts with __systemd__ and other init systems

(C) Martin Väth (martin at mvath.de).
The license of this package is the GNU Public License GPL-2.

This script has been splitted from the **squash_dir** project

https://github.com/vaeth/squash_dir/

because the latter project has essentially been replaced by __squashmount__

https://github.com/vaeth/squashmount/

except for the openrc-wraper which is of independent interest and which
will be further developed only here - the version in **squash_dir** is frozen.
Moreover, some independent __systemd__ init-scripts are provided here.

__openrc-wrapper__ works only for relatively simple __openrc__ initscripts:
If the initscripts are simple enough, __openrc-wrapper__ might even be able to
start/stop them even if __openrc__ is not installed at all (although the
output might look better if __openrc__ is installed).

## Usage

The usage is extremely simple: If you have an __openrc__ init script

`/etc/init.d/`_SERVICE_

possibly with a configuration file

`/etc/conf.d/`_SERVICE_

you just start/stop this service by calling

`/bin/openrc-wrapper` _SERVICE_ `start`

or

`/bin/openrc-wrapper` _SERVICE_ `stop`

respectively. Note that this does not really start/stop the service in the
sense of __openrc__ but just executes the corresponding function of the
initscript (in an environment which somewhat emulates the __openrc__
environment).
In addition, if the __openrc__ init script registered additional
_COMMANDS_ like `store`/`restore`/... you can also use:

`openrc-wrapper` _SERVICE_ _COMMAND_

## Example calls from the command line
-	`openrc-wrapper alsasound save`
-	`openrc-wrapper alsasound restore`

## Examples for systemd

A typical systemd service using an openrc initscript will contain

`ExecStart=/bin/openrc-wrapper` _SERVICE_ `start`

and possibly also

`ExecStop=/bin/openrc-wrapper` _SERVICE_ `stop`

Some examples are in the provided `systemd/system` folder.

## Installation

For installation, just copy `bin/*` into `/bin` and,
at your discretion, the provided __systemd__ initscripts `systemd/system/*`
into your systemd unit folder. In order to get __zsh completion__ support,
also copy `zsh/_openrc-wrapper` somewhere into your zsh's `$fpath`.
Thus, a typical manual installation looks like this
(after you did `cd` into the project's directory):
```
cp bin/* /bin
cp systemd/system/* "`pkg-config --variable=systemdsystemunitdir systemd`"
cp zsh/* /usr/share/zsh/site-functions
```
(For gentoo there is an ebuild in the mv overlay which does this).

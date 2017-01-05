# sudo_mitm #

A simple tool for harvesting sysadmin credentials in Linux

**Overview**

This is a simple shell script that takes advantage of the SUDO\_ASKPASS functionallity in _sudo_ to grab a target users password and forward it on with the help of netcat. From the _sudo_ man page:

	Normally, if sudo requires a password, it will read it from the user's terminal. If
	the -A (askpass) option is specified, a (possibly graphical) helper program is 
	executed to read the user's password and output the password to the standard output.
	If the SUDO_ASKPASS environment	variable is set, it specifies the path to the helper
	program.

**Pre-requisite**

This tool requires the ability to write to the target users home directory. As such, this tool falls entirely under the "post-exploitation" catagory. (Sorry, no "hella 1337 hax" here kids.)

**Usage**

Set the following alias and env variable in one of the users startup scripts, such as the .profile file:

	export SUDO_ASKPASS="$HOME/.sudo_mitm.sh"
	alias sudo='sudo -A'
	
Then place the script on the system and wait for the user to log in. Make sure the script is in the same place the SUDO_ASKPASS variable was just set to.

During one pentest we purposfully downed a key service on the compromised host in order to force the sysadmin to login and use sudo to restart it. Compromising a system on the network is nice. Compromising the sysadmins credentials themselves is even better.

**How to fix?**

I know the blue team readers are probablly wondering how to shut this off. I don't know of a way to do that currently. IMHO the correct fix would be for the sudoers file to specify a new "Defaults" entry type called askpass. Then you could allow for askpass functionallity on your user's graphical desktop machines, yet still deny it's use entirely on production servers. (Of course, the sysads should have different credentials for these different domains, but that is a separate issue.) The theoretical sudoers entry would probablly look something like this:

	Defaults !askpass

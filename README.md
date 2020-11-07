# secure-vnc
A helper library for SSH connections &amp; SSH Tunneled VNC connections

## Why?
Do you find yourself always forgetting the proper syntax or even how to:
1. SSH?
2. SSH with port forwarding?
3. Remote code execution over SSH?
4. Forward VNC connections over SSH?

If so, this repository simplifies your life!  It will ask you questions for each piece of information needed and then build & execute the commands for you!  It will print out the commands and configuration for you in the stdout in case you need it later.  

If you are working with VNC, you know that it is inheritly insecure and all data is sent in cleartext.  The primary solution is to configure the VNC server to only accept VNC requests on the loopback address (localhost | 127.0.0.1) and tunnel the VNC connection through an SSH Tunnel.  This repository automates this task for you taking your time from 15min to seconds.  

Prefer not to have VNC automatically running on boot?  No problem!  This script is designed for that specific usecase.  

## HOW IT WORKS

### SSH
Secure-vnc prompts the user for input of parameters specific to creating a basic SSH TTY if the parameters are not already provided via command line switches or environmental variables.  Once all parameters are filled, it prints out what the command syntax is and then uses it unless the `-T` option is used.  This option sets the script into dry-run mode to output the necessary syntax and establish environmental variables.

### VNC
Secure-vnc prompts the user for input of parameters for creating a connection if the parameters are not already provided via command line switches or environmental variables.  A dynamically generated SSH configuration is generated from the prompts in the current working directory.  Before building a tunnel, the script connects over SSH to query the system if any VNC displays are currently active `vncserver -list -cleanstale`.  At this time, this command is specific to TigerVNC only.

If none are found, secure-vnc spawns a new display, detects the port & display number from the output, and configures a new ssh tunnel for the specific port.  Lastly, it  requests your local computer to open the default application that supports a vnc URL.

If 1 display is found, secure-vnc prompts the user if you would like to resume your session or spawn a new desktop.  Resuming your desktop means the script will automatically determine port tunneling to then open your vncviewer to the correct address.  If you decide for a new one, it acts as though no other displays were found and executes accordingly.

Lastly, if more than one display is found, secure-vnc prompts the user for a choice of any of the desktops or to create a new one.  The same thing happens here as above.

#### CAVIOTS:
- If you have local SSH Configurations, they will be ignored as the SSH config is generated and provided directly to the SSH command.
- `*-local` scripts will append `.local` onto a hostname if it is not provided.  Local IPs are accepted as well.
- `*-remote` scripts do not support domain name lookups and remote IP is required.

## PRE-REQUISTES
These scripts are placed in the proper folder for which technology they require.  Currently supports bash execution on Mac OSX & Linux for SSH & VNC capabilities.   This does NOT include WSL 2 or CYGWIN at this time, support unknown.
#### Local Host
- SSH (public key usage)
- [OPTIONAL] `users` file in current working directory\*
- Default application for opening vnc uri's `localhost:[port]` configured.\*\*

    Notes:
    
    \*File follows bash syntax for defining a USERS variable which is an array of usernames.  This helps remind the user which usernames are defined on the remote host per project.
    
    \*\*Secure-vnc uses `open` or `vncviewer` for Mac OSX and Linux respectfully to spawn the default vnc application using a vnc protocol URI.  An error will occur if there are no default applications installed for the VNC protocol. Mac OSX comes with `Screen Sharing.app` by default.  I recommend installing `tigervnc-viewer` for Linux.

#### Remote Host
- Pre-configured user's `~/.ssh/authorized_keys` file with matching SSH `*.pub` key
- TigerVNC (not required to be running on startup)

## INSTALLATION

```sh
## Grab latest published version
$ VERSION="$(curl -Lo- "https://api.github.com/repos/codejedi365/secure-vnc/releases/latest" | \
    grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')"

## Download latest version
$ curl -L -o "secure-vnc.tar.gz" "https://github.com/codejedi365/secure-vnc/archive/${VERSION}.tar.gz"

## Unpack Download
$ tar --chroot -xvt ./secure-vnc.tar.gz

## Enable SSH scripts for use
$ chmod +x $PROJECT_DIR/bash/ssh-* 

## Enable VNC scripts for use
$ chmod +x $PROJECT_DIR/bash/vnc-*
```

## USE
There are 4 scripts currently to use.  Your choice is dependent on where your remote host is located.  If you want to use just a hostname on the Local Area Network (LAN) then the `*-local` scripts provide dedicated help and prompts for that.  If you only have the IP address then use the `*-remote` scripts.
```sh
## [ALL] Go to project directory
$ cd $PROJECT_DIR

# CHOOSE 1 of these options:
## [1] For SSH LAN connections
$ ./bash/ssh-local

## [2] For SSH WAN connections
$ ./bash/ssh-remote

## [3] For VNC over LAN connections
$ ./bash/vnc-local

## [4] For VNC over WAN connections
$ ./bash/vnc-remote
```

## SUPPORTED ENVIRONMENTAL VARIABLES
If any of the following variables are defined when the script is run then the script will use those variables and not ask the user for input.
- REMOTE_ADDRESS ------------ ip address
- IDENTITY_KEY ----------------- ssh private key filepath
- DEFAULT_USER ---------------- username on remote host to login as over SSH
- DEFAULT_rHOSTNAME -------- (local only) hostname
- DEFAULT_SSH_PORT ---------- remote host port of ssh server (sshd.service)
- DEFAULT_VNC_PORT_LOCAL -- base port to start at when port forwarding
- DEFAULT_VNC_PORT ---------- remote base port to start at when requesting vnc connections

## FUTURE GOALS & COMING SOON
1. **[HELP REQUESTED]** Port over to Python & handle cross-platform to include Windows
2. Plugin architecture for other vncserver applications
3. Management ability to kill other displays (this will loose any unsaved work)

## CONTRIBUTE

### VOTE for requirements you want to see fulfilled
- Is it desired to pull in local ssh configuration settings? <!-- #1 -->

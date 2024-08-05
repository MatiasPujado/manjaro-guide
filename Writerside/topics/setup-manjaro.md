# Setting up Manjaro

## Setup Nautilus columns

Under **/org/gnome/nautilus/list-view/default-visible-columns** add as custom value:

```Bash
['name', 'size', 'owner', 'group', 'permissions', 'date_modified', 'detailed_type', 'starred']
```

Under **/org/gnome/nautilus/list-view/default-column-order** add as custom value:

```Bash
['name', 'size', 'owner', 'group', 'permissions', 'date_modified_with_time', 'date_accessed', 'date_created', 'recency', 'type', 'mime_type', 'where', 'date_modified', 'starred']
```

## System Environment Variables {collapsible="true"}

Adapt the following example to your needs and add it to the `/etc/environment` file.

```Bash
sudo vim /etc/environment
```

```text
#
# This file is parsed by pam_env module
#
# Syntax: simple "KEY=VAL" pairs on separate lines
#
# Manjaro defaults are now in /etc/profile.d/
#

EDITOR=/usr/bin/vim

# https://doc.qt.io/qt-5/highdpi.html#high-dpi-support-in-qt
QT_AUTO_SCREEN_SCALE_FACTOR=1

# To use QGnomePlaform with AdwaitaQt style, comment out the following keys:

# Available options: gtk2, gnome, kde, qt5ct, xcb

# Note: Don't use in combination with QT_STYLE_OVERRIDE
QT_QPA_PLATFORMTHEME="qt5ct"

# Available styles: HighContrastInverse, HighContrast, Adwaita-HighContrastInverse,
# Adwaita-HighContrast, Adwaita-Dark, Adwaita, kvantum-dark, kvantum, qt5ct-style,
# Windows, Fusion

# Note: Don't use in combination with QT_QPA_PLATFORMTHEME
#QT_STYLE_OVERRIDE=""

## JAVA
JAVA_HOME=/usr/lib/jvm/jdk1.8.0_411
JRE_HOME=/usr/lib/jvm/jdk1.8.0_411/jre
JAVAFX=/opt/javafx/jdk-17.0.11

## MAVEN
MAVEN_HOME=/opt/apache-maven-3.9.8
M2_HOME=$HOME/.m2/wrapper/dists/

## GRADLE
GRADLE_HOME=/opt/gradle-8.8

## TOMCAT
CATALINA_HOME=/opt/apache-tomcat-9.0.80
CATALINA_BASE=/opt/apache-tomcat-9.0.80

## GO
GOPATH=/opt/go
GOROOT=/opt/go

## PYTHON
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python

## ORACLE DB
ORACLE_HOME=/opt/oracle
LD_LIBRARY_PATH=$ORACLE_HOME/instantclient

## CITRIX
ICAROOT=/opt/Citrix/ICAClient

## VSCODE
VSCODE_HOME=/opt/vscode/vscode_cli

## NODE
NODE_HOME=$HOME/.config/nvm/versions/node/v18.20.3

## ROCM
ROCM_PATH=/opt/rocm

## SYSTEM
CGO_ENABLED=0
RADV_PERFTEST=aco

PATH=/usr/local/sbin:/usr/local/bin:/usr/bin:/usr/sbin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/local/mysql/bin:$JAVA_HOME/bin:$JRE_HOME/bin:$JAVAFX/lib:$JAVAFX/jmods:$JAVAFX/javadoc:$MAVEN_HOME/bin:$M2_HOME/bin:$GRADLE_HOME/bin:$ORACLE_HOME:$ORACLE_HOME/sqldeveloper:$ORACLE_HOME/sqldeveloper/sqldeveloper/bin:$LD_LIBRARY_PATH:$CATALINA_BASE:$CATALINA_HOME/bin:$GOPATH/bin:$GOROOT/bin:$VIRTUALENVWRAPPER_PYTHON:$ICAROOT:$VSCODE_HOME:$NODE_HOME/bin
```

## Set system-wide permissions {collapsible="true"}

<br/>

> **Warning**
>
> Be careful when editing the sudoers file. Incorrect syntax can lock you out of your system. Always use the `visudo` command to edit the sudoers file. This command checks the
syntax of the file before saving it.
>
{style="warning"}

<br/>

> **Note**
>
>If visudo does not open with the desired text editor, look for this line in /etc/sudoers and comment it out:
>
>```text
>Defaults!/usr/bin/visudo env_keep += "SUDO_EDITOR EDITOR VISUAL"
>```
> A less permanent way to fix this is to run the following command:
>```text
>sudo EDITOR=/usr/bin/vim visudo -f /etc/sudoers.d/sudoers
>```
>
{style="note"}

<br/>

This is my sudoers file, it is not ideal, but it suits my needs. **Do not copy it blindly and adapt it to your needs.**

<br/>

```Bash
sudo visudo -f /etc/sudoers.d/sudoers
```

```text
## sudoers file.
##
## This file MUST be edited with the 'visudo' command as root.
## Failure to use 'visudo' may result in syntax or file permission errors
## that prevent sudo from running.
##
## See the sudoers man page for the details on how to write a sudoers file.
##

##
## Host alias specification
##
## Groups of machines. These may include host names (optionally with wildcards),
## IP addresses, network numbers or netgroups.
# Host_Alias	WEBSERVERS = www1, www2, www3

##
## User alias specification
##
## Groups of users.  These may consist of user names, uids, Unix groups,
## or netgroups.
# User_Alias	ADMINS = millert, dowdy, mikef

##
## Cmnd alias specification
##
## Groups of commands.  Often used to group related commands together.
# Cmnd_Alias	PROCESSES = /usr/bin/nice, /bin/kill, /usr/bin/renice, \
# 			    /usr/bin/pkill, /usr/bin/top
#
# Cmnd_Alias	REBOOT = /sbin/halt, /sbin/reboot, /sbin/poweroff
#
# Cmnd_Alias	DEBUGGERS = /usr/bin/gdb, /usr/bin/lldb, /usr/bin/strace, \
# 			    /usr/bin/truss, /usr/bin/bpftrace, \
# 			    /usr/bin/dtrace, /usr/bin/dtruss
#
# Cmnd_Alias	PKGMAN = /usr/bin/apt, /usr/bin/dpkg, /usr/bin/rpm, \
# 			 /usr/bin/yum, /usr/bin/dnf,  /usr/bin/zypper, \
# 			 /usr/bin/pacman

##
## Defaults specification
##
## Preserve editor environment variables for visudo.
## To preserve these for all commands, remove the "!visudo" qualifier.
## Defaults!/usr/bin/visudo env_keep += "SUDO_EDITOR EDITOR VISUAL"
##
## Use a hard-coded PATH instead of the user's to find commands.
## This also helps prevent poorly written scripts from running
## artbitrary commands under sudo.
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/bin"
##
## You may wish to keep some of the following environment variables
## when running commands via sudo.
##
## Locale settings
# Defaults env_keep += "LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET"
##
## Run X applications through sudo; HOME is used to find the
## .Xauthority file.  Note that other programs use HOME to find   
## configuration files and this may lead to privilege escalation!
# Defaults env_keep += "HOME"
##
## X11 resource path settings
# Defaults env_keep += "XAPPLRESDIR XFILESEARCHPATH XUSERFILESEARCHPATH"
##
## Desktop path settings
# Defaults env_keep += "QTDIR KDEDIR"
##
## Allow sudo-run commands to inherit the callers' ConsoleKit session
# Defaults env_keep += "XDG_SESSION_COOKIE"
##
## Uncomment to enable special input methods.  Care should be taken as
## this may allow users to subvert the command being run via sudo.
# Defaults env_keep += "XMODIFIERS GTK_IM_MODULE QT_IM_MODULE QT_IM_SWITCHER"
##
## Uncomment to restore the historic behavior where a command is run in
## the user's own terminal.
# Defaults !use_pty
##
## Uncomment to send mail if the user does not enter the correct password.
# Defaults mail_badpass
##
## Uncomment to enable logging of a command's output, except for
## sudoreplay and reboot.  Use sudoreplay to play back logged sessions.
## Sudo will create up to 2,176,782,336 I/O logs before recycling them.
## Set maxseq to a smaller number if you don't have unlimited disk space.
# Defaults log_output
# Defaults!/usr/bin/sudoreplay !log_output
# Defaults!/usr/local/bin/sudoreplay !log_output
# Defaults!REBOOT !log_output
# Defaults maxseq = 1000
##
## Uncomment to disable intercept and log_subcmds for debuggers and
## tracers.  Otherwise, anything that uses ptrace(2) will be unable
## to run under sudo if intercept_type is set to "trace".
# Defaults!DEBUGGERS !intercept, !log_subcmds
##
## Uncomment to disable intercept and log_subcmds for package managers.
## Some package scripts run a huge number of commands, which is made
## slower by these options and also can clutter up the logs.
# Defaults!PKGMAN !intercept, !log_subcmds

##
## Runas alias specification
##

##
## User privilege specification
##
root ALL=(ALL:ALL) ALL

## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL:ALL) ALL

## Same thing without a password
# %wheel ALL=(ALL:ALL) NOPASSWD: ALL

## Uncomment to allow members of group sudo to execute any command
# %sudo	ALL=(ALL:ALL) ALL
%sudo	ALL=(ALL:ALL) ALL
%dev	ALL=(ALL:ALL) ALL

## Uncomment to allow any user to run sudo if they know the password
## of the user they are running the command as (root by default).
# Defaults targetpw  # Ask for the password of the target user
# ALL ALL=(ALL:ALL) ALL  # WARNING: only use this together with 'Defaults targetpw'

## Read drop-in files from /etc/sudoers.d
# @includedir /etc/sudoers.d
admin ALL=(ALL:ALL) NOPASSWD: ALL
matias ALL=(ALL:ALL) ALL
coder ALL=(ALL:ALL) ALL
```

## Setup users accounts

### Create new groups

```Bash
sudo groupadd dev
sudo groupadd sudo
```

### Add user to groups

Adapt the following example to your needs.

```Bash
sudo usermod -aG sudo,dev,matias,coder,admin,audio,input,lp,storage,users,network,power,realtime coder
sudo usermod -aG sudo,dev,matias,coder,admin,audio,input,lp,storage,users,network,power,realtime matias
sudo usermod -aG sudo,dev,matias,coder,admin,audio,input,lp,storage,users,network,power,realtime admin
```

### Change users main group

To keep my accounts organized, I change the main group of the users to the `dev` group.

```Bash
sudo usermod -g dev coder
sudo usermod -g dev matias
sudo usermod -g dev admin
```

## Mount partitions

This section is about mounting partitions in the `/etc/fstab` file. An alternative is to use the `Disks` application, especially if they weren't included during the OS installation
process.

First, we need to identify the partitions we want to mount. Use the `lsblk` command to list all the block devices:

```Bash
lsblk
```

Then we will use the `blkid` command to get the UUID of the partition:

```Bash
sudo blkid
```

Now we can edit the `/etc/fstab` file:

```Bash
sudo vim /etc/fstab
```

<br/>

> **Note**
>
> In the last line of the '/etc/fstab' file, I am mounting a shared folder from another Linux system in my network.
>
{style="note"}

<br/>

This is an example of how to add a partition to the `/etc/fstab` file:

```text
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# systemd generates mount units based on this file, see systemd.mount(5).
# Please run 'systemctl daemon-reload' after making changes here.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/nvme0n1p3 during installation
UUID=2a1db98a-de36-4491-b799-e7fd9607498a /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/nvme0n1p1 during installation
UUID=5F4A-0896  /boot/efi       vfat    umask=0077      0       1
# /home was on /dev/nvme0n1p4 during installation
UUID=66afd3f0-a5da-4c6e-a30d-f3f20fa02e83 /home           ext4    defaults        0       2
# swap was on /dev/nvme0n1p2 during installation
UUID=f7958b58-76ef-4824-9b3f-7c2131390585 none            swap    sw              0       0

#
# LVM
# Media_VG
# MediaCenter with UUID="daf6302c-dd08-4e2e-a7f1-24a9511bbf93"
UUID="daf6302c-dd08-4e2e-a7f1-24a9511bbf93"	/media/MediaCenter	ext4	defaults	0	0
# VMs with UUID="a28a5163-da8c-4791-843d-5f5fced9b4db"
UUID="a28a5163-da8c-4791-843d-5f5fced9b4db"	/media/VMs	ext4	defaults	0	0
#
#Standard Partitions
# Games with UUID="a90d6e24-6af9-4b14-937e-b359614fb787"
UUID="a90d6e24-6af9-4b14-937e-b359614fb787"	/media/Games	ext4	defaults	0	0
# Vault with UUID="a9be72c3-3142-43f8-a419-ca6b2ff52c0f"
/dev/sdc1	/media/Vault	ext4	defaults	0	0
# Work with UUID="5e255cc3-1733-45e0-a709-a0174a417f6b"
/dev/sdd1	/media/Work	ext4	defaults	0	0
# Dell Shared-Folder
\\192.168.100.100\SharedFolder  /media/Dell  cifs  credentials=/etc/win-credentials,uid=1001,gid=1001,dir_mode=0774,file_mode=0774 0       0
```

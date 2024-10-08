# Home Servers

## Printers {collapsible="true"}

### Brother DCP-1617NW

Install the necessary packages and other dependencies:

```Bash
sudo pacman -S --needed cups manjaro-printer gtk2 sane lib32-glibc system-config-printer
```

Then, install the Brother DCP-1617NW drivers:

```Bash
sudo pamac install brscan4 brother-dcp1610w brother-lpr-drivers-common
```

Add the printer to the system:

```Bash
sudo brsaneconfig4 -a name=Brother model="DCP-1617NW" ip=192.168.100.150
```

## Plex Media Server {collapsible="true"}

Use the following command to install Plex Media Server:

```Bash
yay -S --needed plexmediaserver
```

### Add users to Plex group

```Bash
sudo usermod -aG plex admin
sudo usermod -aG plex matias
sudo usermod -aG plex coder
```

### Enable and start the service

```Bash
sudo systemctl enable ---now plexmediaserver.service
```

### Fix ulimit and set security settings

```Bash
sudo gedit /etc/systemd/system/plexmediaserver.service.d/restrict.conf
```

```Generic
[Service]
ReadOnlyPaths=/
ReadWritePaths=/var/lib/plex /tmp
MemoryMax=4G
```

### Fix max amount of libraries for non-root users

```Bash
sudo gedit /etc/sysctl.d/40-max-user-watches.conf
```

```Generic
fs.inotify.max_user_watches=524288
```

## FTP Server {collapsible="true"}

### Create a new FTP user account

sudo useradd -m ftpuser
sudo passwd ftpuser

Deleting ftp user account
sudo userdel ftpuser
sudo userdel -r ftpuser



Hide ftpuser from login screen
sudo gedit /var/lib/AccountsService/users/ftpuser
[User]
Languages=en_US.UTF-8;
Session=
Icon=/home/ftpuser/.face
SystemAccount=true


### Update servers configuration

First, backup the original configuration file:

```Bash
sudo mv /etc/vsftpd.conf /etc/vsftpd.conf.bak
```

Then, create a new configuration file and edit it:

```Bash
sudo touch /etc/vsftpd.conf
sudo vim /etc/vsftpd.conf
```

```Generic
# Example config file /etc/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
# Allow anonymous FTP? (Beware - allowed by default if you comment this out).
anonymous_enable=NO
#
# Uncomment this to allow local users to log in.
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
#local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
ascii_upload_enable=YES
ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
chroot_local_user=NO
chroot_list_enable=YES
# (default follows)
chroot_list_file=/etc/vsftpd.chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
ls_recurse_enable=YES
#
# When "listen" directive is enabled, vsftpd runs in standalone mode and
# listens on IPv4 sockets. This directive cannot be used in conjunction
# with the listen_ipv6 directive.
listen=YES
#
# This directive enables listening on IPv6 sockets. To listen on IPv4 and IPv6
# sockets, you must run two copies of vsftpd with two configuration files.
# Make sure, that one of the listen options is commented !!
#listen_ipv6=YES

# Set own PAM service name to detect authentication settings specified
# for vsftpd by the system package.
pam_service_name=vsftpd

# add to the end : specify chroot directory
# if not specified, users' home directory equals FTP home directory
#local_root=/media/Vault
# turn off seccomp filter if cannot login normally
seccomp_sandbox=NO
allow_writeable_chroot=YES
guest_enable=YES
# Imports different user profiles (/etc/user_conf/{coder,admin,matias,ftpuser}) with different directories
user_config_dir=/etc/user_conf
```

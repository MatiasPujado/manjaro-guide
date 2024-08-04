# Network Settings

DNS
GOOGLE
8.8.8.8, 8.8.4.4

2001:4860:4860:0:0:0:0:8888, 2001:4860:4860:0:0:0:0:8844

CLOUDFLARE
https://developers.google.com/speed/public-dns/docs/using
https://developers.cloudflare.com/1.1.1.1/setup/linux/

1.1.1.1, 1.0.0.1

2606:4700:4700:0:0:0:0:1111, 2606:4700:4700:0:0:0:0:1001
​​
Block malware

1.1.1.2, 1.0.0.2

2606:4700:4700:0:0:0:0:1112, 2606:4700:4700:0:0:0:0:1002
​​
Block malware and adult content

1.1.1.3, 1.0.0.3

2606:4700:4700:0:0:0:0:1113, 2606:4700:4700:0:0:0:0:1003


FIREWALL UFW
https://userbase.kde.org/KDEConnect#

UFW Rules

sudo pacman -S --needed ufw gufw

sudo ufw status numbered
sudo ufw status verbose
sudo ufw enable
sudo ufw disable
sudo ufw logging high

Rules

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow http
sudo ufw allow https
sudo ufw allow log ssh
sudo ufw allow out log ssh
sudo ufw allow log ftp
sudo ufw allow out log ftp
sudo ufw allow log CIFS
# For torrents
sudo ufw allow log 56881:56889/tcp
# For Steam
sudo ufw allow 27000:27100/udp
sudo ufw allow 27031:27036/udp
sudo ufw allow 27036:27037/tcp
sudo ufw allow 4380/udp
sudo ufw allow 3478/udp
sudo ufw allow 4379/udp
sudo ufw allow 27014:27030/udp
sudo ufw allow 27015/tcp
sudo ufw allow 27015/udp
# KDE-Connect
sudo ufw allow 1714:1764/udp
sudo ufw allow 1714:1764/tcp

sudo gedit /etc/ufw/applications.d/samba

'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
[Samba]
title=LanManager-like file and printer server for Unix
description=The Samba software suite is a collection of programs that implements the SMB/CIFS protocol for unix systems, allowing you to serve files and printers to Windows, NT, OS/2 and DOS clients. This protocol is sometimes also referred to as the LanManager or NetBIOS protocol.
ports=137,138/udp|139,445/tcp
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

sudo gedit /etc/ufw/applications.d/plexmediaserver

'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
[plexmediaserver]
title=Plex Media Server (Standard)
description=The Plex Media Server
ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp

[plexmediaserver-dlna]
title=Plex Media Server (DLNA)
description=The Plex Media Server (additional DLNA capability only)
ports=1900/udp|32469/tcp

[plexmediaserver-all]
title=Plex Media Server (Standard + DLNA)
description=The Plex Media Server (with additional DLNA capability)
ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp|1900/udp|32469/tcp
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

sudo ufw app update samba
sudo ufw allow log samba
sudo ufw app update plexmediaserver
sudo ufw allow plexmediaserver-all
sudo ufw reload

Apps

$ sudo ufw app list



How-to delete rules

$ sudo ufw status numbered
$ sudo ufw delete [number]

Reset

$ sudo ufw reset

# Network Settings

## DNS

DNS (Domain Name System) is a system that translates domain names to IP addresses. DNS servers are used to resolve domain names to IP addresses. The DNS server is responsible for
resolving domain names to IP addresses. The DNS server is responsible for resolving domain names to IP addresses.

### Google IPv4

```Generic
8.8.8.8, 8.8.4.4
```

### Google IPv6

```Generic
2001:4860:4860:0:0:0:0:8888, 2001:4860:4860:0:0:0:0:8844
```

### Cloudflare IPv4

```Generic
1.1.1.1, 1.0.0.1
```

### Cloudflare IPv6

```Generic
2606:4700:4700:0:0:0:0:1111, 2606:4700:4700:0:0:0:0:1001
```

### Cloudflare IPv4 - Block malware

```Generic
1.1.1.2, 1.0.0.2
```

### Cloudflare IPv6 - Block malware

```Generic
2606:4700:4700:0:0:0:0:1112, 2606:4700:4700:0:0:0:0:1002
```

### Cloudflare IPv4—Block malware and adult content

```Generic
1.1.1.3, 1.0.0.3
```

### Cloudflare IPv6—Block malware and adult content

```Generic
2606:4700:4700:0:0:0:0:1113, 2606:4700:4700:0:0:0:0:1003
```

## FIREWALL UFW

UFW is a firewall configuration tool for iptables. UFW is a user-friendly way to create an IPv4 or IPv6 host-based firewall. By default, UFW is disabled.

### Install UFW and GUI

```Bash
sudo pacman -S --needed ufw gufw
```

### UFW Commands

```Bash
sudo ufw status numbered
```

```Bash
sudo ufw status verbose
```

```Bash
sudo ufw enable
```

```Bash
sudo ufw disable
```

```Bash
sudo ufw logging high
```

### UFW Rules

```Bash
sudo ufw default deny incoming
```

```Bash
sudo ufw default allow outgoing
```

```Bash
sudo ufw allow http
```

```Bash
sudo ufw allow https
```

```Bash
sudo ufw allow log ssh
```

```Bash
sudo ufw allow out log ssh
```

```Bash
sudo ufw allow log ftp
```

```Bash
sudo ufw allow out log ftp
```

```Bash
sudo ufw allow log CIFS
```

#### For torrents

```Bash
sudo ufw allow log 56881:56889/tcp
```

#### For Steam

```Bash
sudo ufw allow 27000:27100/udp
```

```Bash
sudo ufw allow 27031:27036/udp
```

```Bash
sudo ufw allow 27036:27037/tcp
```

```Bash
sudo ufw allow 4380/udp
```

```Bash
sudo ufw allow 3478/udp
```

```Bash
sudo ufw allow 4379/udp
```

```Bash
sudo ufw allow 27014:27030/udp
```

```Bash
sudo ufw allow 27015/tcp
```

```Bash
sudo ufw allow 27015/udp
```

#### KDE-Connect

```Bash
sudo ufw allow 1714:1764/udp
```

```Bash
sudo ufw allow 1714:1764/tcp
```

#### Samba

```Bash
sudo vim /etc/ufw/applications.d/samba
```

```Bash
[Samba]
title=LanManager-like file and printer server for Unix
description=The Samba software suite is a collection of programs that implements the SMB/CIFS protocol for unix systems, allowing you to serve files and printers to Windows, NT, OS/2 and DOS clients. This protocol is sometimes also referred to as the LanManager or NetBIOS protocol.
ports=137,138/udp|139,445/tcp
```

```Bash
sudo ufw app update samba
```

```Bash
sudo ufw allow log samba
```

#### Plex Media Server

```Bash
sudo vim /etc/ufw/applications.d/plexmediaserver
```

```Bash
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
```

```Bash
sudo ufw app update plexmediaserver
```

```Bash
sudo ufw allow plexmediaserver-all
```

```Bash
sudo ufw reload
```

### Get the list of applications

```Bash
sudo ufw app list
```

### Delete rules

```Bash
sudo ufw delete [number]
```

### Reset UFW

```Bash
sudo ufw reset
```

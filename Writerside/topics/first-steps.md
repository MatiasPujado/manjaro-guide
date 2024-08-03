# First Steps

## On a fresh install

Update mirrors database and upgrade the system.

```Bash
sudo pacman-mirrors -f && sudo pacman -Syyu
```

## Install packages from backup files {collapsible="true"}

To reinstall the packages, use the following command:

```Bash
sudo pacman -S --needed - < pkglist
cat aur-pkglist | xargs -I {} pamac build {}
```

## Install important dependencies

```Bash
sudo pacman -S --needed base-devel wget curl nano mercurial make dnsmasq vim dialog meson sane samba gawk lsp-plugins lsp-plugins-docs mda.lv2 calf qjournalctl yay qt5pas libappindicator-gtk3 openssh xorg-xhost smartmontools nvme-cli deja-dup zenity cmake gvfs qt5-wayland eog lib32-glibc alsa-lib gst-plugins-base-libs gtk2 libc++ libc++abi libidn11 libjpeg6-turbo libpng12 libsecret libsoup libvorbis libxaw libxp openssl speex webkit2gtk libc++ xerces-c apparmor qt5-svg
```

## Install essential utilities

```Bash
sudo pacman -S --needed gparted grsync corectrl easyeffects font-manager shotwell terminator discord conky transmission-gtk frotz-ncurses bitwarden filezilla paperwork openshot gedit netstat-nat manjaro-pipewire pipewire-jack pipewire-v4l2 pipewire-docs realtime-privileges wireplumber tree vsftpd dconf-editor git git-sizer git-repair git-revise git-filter-repo zsh zsh-doc lsd awesome-terminal-fonts nerd-fonts tmux
```

## TRIM (only on SSD)

> **Note**
>
> It enables an operating system (OS) to inform a NAND flash solid-state drive (SSD) which data blocks it can erase because they are no longer in use. The use of this command can improve the performance of writing data to SSDs and contribute to longer life for the SSD.
>
{style="note"}

```Bash
sudo systemctl enable --now fstrim.timer
systemctl status fstrim.timer
```

## Timeshift

```Bash
sudo pacman -S --needed timeshift timeshift-autosnap-manjaro
```

#### Enable autosnapshots

Might give you an answer if dead or disabeled.

```Bash
systemctl status cronie
```

```Bash
sudo systemctl enable --now cronie.service
```

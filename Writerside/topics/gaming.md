# Gaming on Linux

## Lutris

Lutris is a gaming platform that allows you to install, configure, and launch games from one place. It supports a wide range of games, including those from GOG, Steam and Humble.

```Bash
sudo pacman -S --needed lutris
```

## Heroic Games Launcher

Heroic Games Launcher is an open-source alternative to the Epic Games Launcher. It allows you to install, configure, and launch games from the Epic Games Store.

```Bash
yay -S --needed heroic-games-launcher-bin
```

## Steam

Steam is a digital distribution platform that allows you to buy, download, and play games on Linux. It is the most popular gaming platform on Linux and supports a wide range of
games.

```Bash
sudo pacman -S --needed steam linux-steam-integration steam-native-runtime
```

## Wine (Wine Is Not an Emulator)

Wine is a compatibility layer that allows you to run Windows applications on Linux. It is not an emulator but rather a translation layer that converts Windows API calls into POSIX
calls on-the-fly. This allows you to run Windows applications on Linux without the need for a virtual machine or dual-booting.

```Bash
sudo pacman -S --needed wine-staging winetricks wine-gecko wine-mono mono vkd3d lib32-vkd3d wine-gecko
```

Configuring Wine for x64:

```Bash
winecfg
```

Configuring Wine for x86:

Enable these environment variables in your shell:

```Bash
export WINEARCH=win32
export WINEPREFIX=~/.wine32
```

And create the directory:

```Bash
mkdir -p ~/.wine32/drive_c
```

```Bash
winecfg
```

### Winetricks

Winetricks is a script that can be used to install various libraries and dependencies required by Windows applications to run on Linux.

```Bash
winetricks
```

Install dependencies in one go:

```Bash
winetricks corefonts lucida droid consolas allcodecs dxvk dotnet40 d3dx9_43
```

## Proton

Proton is a compatibility layer that allows you to run Windows games on Linux. It is a fork of Wine with additional patches and tools to make it easier to run games on Linux.

### Proton Manager

#### Install ProtonUp-QT from AUR repository

```Bash
yay -S --needed protonup-qt
```

#### Install Protontricks from AUR repository

A simple wrapper that does winetricks things for Proton-enabled games.

```Bash
yay -S --needed protontricks
```

#### Install ProtonUp CLI

ProtonUp-RS from AUR repository:

```Bash
wget https://github.com/auyer/Protonup-rs/releases/latest/download/protonup-rs-linux-amd64.tar.gz -O - | tar -xz && zenity --password | sudo -S mv protonup-rs /usr/bin/
```

ProtonUp-NG from AUR repository:

```Bash
yay -S --needed protonup-ng-git
```

#### ProtonUp-NG using Python

```Bash
pip3 install protonup-ng
```

Uninstall ProtonUp-NG using Python:

```Bash
pip3 uninstall protonup-ng
```

Add these lines to your shell configuration file:

```Bash
export PATH=$PATH:~/.local/bin
source .zshrc
```

#### Check Protonup version installed

```Bash
pip list | grep protonup
```

Before opening Steam, set your installation directory:

```Bash
protonup -d "~/.steam/root/compatibilitytools.d/"
```

#### Update to the latest version

```Bash
protonup
```

#### List available versions

```Bash
protonup --releases
```

#### Install a specific version

```Bash
protonup -t 6.5-GE-2
```

#### List existing installations

```Bash
protonup -l
```

#### Remove existing installations

```Bash
protonup -r 6.5-GE-2
```

#### Download Proton-GE without installing it

```Bash
protonup --download
```

## ESync - Helps Game Overhead

Check to see if Esync is enabled (most distributions do by default)

```Bash
ulimit -Hn
```

If the output is higher than `500,000`, then **ESYNC is enabled**. If the returning value is lower or equal to `500,000`, then **ESYNC is disabled**.
You will have to enable it manually. Change the following files:

```Bash
sudo vim /etc/systemd/system.conf
```

```Bash
DefaultLimitNOFILE=524288
```

> **Note**
>
> Make sure you change `<username>` to your actual username.
>
{style="note"}

```Bash
sudo gedit /etc/security/limits.conf
```

```Bash
<username> hard nofile 524288
<username> soft nofile 524288
```

## GameMode

GameMode is a daemon/lib combo for Linux that allows games to request a set of optimizations to be temporarily applied to the host OS.

```Bash
sudo pacman -S --needed gamemode lib32-gamemode
```

## Goverlay

Goverlay is a graphical tool to manage Linux system settings for gaming. It allows you to configure various settings such as CPU governor, I/O scheduler, and GPU overclocking.

```Bash
yay -S --needed goverlay-bin
```

## Replay Sorcery

Replay is a tool that allows you to record and replay your games on Linux. It supports various formats such as MP4, WebM, and GIF.

```Bash
yay -S --needed replay-sorcery
```

## Gamescope

Gamescope is a compositor that allows you to run games in a separate X server with a custom resolution and refresh rate. This can help improve performance and reduce input lag in
games.

```Bash
sudo pacman -S --needed gamescope
```

## MangoHud

MangoHud is a heads-up display (HUD) for Linux that allows you to monitor FPS, CPU and GPU usage, and other performance metrics while gaming.

```Bash
sudo pacman -S --needed mangohud lib32-mangohud
```

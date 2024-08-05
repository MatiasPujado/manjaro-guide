# Apps

Here is a list of apps that I use daily. I will try to keep this list updated as I find new apps that I like.

## Zathura

```Bash
sudo pacman -S --needed zathura zathura-cb zathura-djvu zathura-pdf-poppler zathura-ps
```

## Web Browsers

```Bash
sudo pacman -S --needed brave-browser firefox firefox-developer-edition libgnome-keyring libsecret
```

## Image Editors

```Bash
sudo pacman -S --needed gimp gimp-help-en gimp-nufraw gimp-plugin-gmic darktable rawtherapee inkscape
```

## Conky

```Bash
sudo pacman -S --needed conky
```

Some Distros require a fix for Conky to work properly. If you are using Garuda, you can use the following command to fix it.

```Bash
sudo mv /etc/xdg/autostart/conky.desktop /etc/xdg/autostart/conky.desktop.bak
```

## Maintenance

```Bash
sudo pacman -S --needed bleachbit
```

## Spotify

```Bash
sudo pacman -S --needed spotify-launcher zenity
```

## AppImageLauncher

```Bash
sudo pacman -S appimagelauncher
```

## VirtualBox

```Bash
sudo pacman -S --needed linux-headers virtualbox-guest-iso virtualbox virtualbox-host-dkms
```

## OBS-Studio

```Bash
sudo pacman -S --needed obs-studio
```

## Screen Recorder

```Bash
sudo pacman -S kooha
```

## Citrix Workspace {collapsible="true"}

### From AUR repository:

> **Warning**
>
> The latest version of Citrix Workspace is not working well on Manjaro. Install it through AUR at your own risk.
>
{style="note"}

```Bash
sudo pamac install icaclient
```

You have to create '$HOME/.ICAClient/cache' for each user who uses this program and then populate it with the appropriate ini files.

```Bash
mkdir -p $HOME/.ICAClient/cache
cp /opt/Citrix/ICAClient/config/{All_Regions,Trusted_Region,Unknown_Region,canonicalization,regions}.ini $HOME/.ICAClient/
```

### From the official website:

> **Important**
>
> The version of Citrix Workspace available that works well with Manjaro is linuxx64-23.11.0.82.tar.gz.
> The tarball package does not do dependency checks nor install dependencies. All system dependencies must be resolved separately.
>
{style="note"}

<br/>

1. Download the tarball package from the official website.
2. Extract the contents of the .tar.gz file into an empty directory.

```Bash
cd ~/.tmp
tar -xvzf linuxx64-23.11.0.82.tar.gz
```

3. Run the setup program with sudo and follow its prompts carefully. Say yes to all the prompts but to `App Protection` unless you need it.

```Bash
sudo ./setupwfc
```

4. If you have previously installed GStreamer, you can choose whether to integrate GStreamer with the Citrix Workspace app and support HDX MediaStream Multimedia Acceleration. To
   integrate Citrix Workspace app with GStreamer, type y at the prompt.

> **Important**
>
> On some platforms, installing the client from a tarball package can cause the system to become unresponsive after prompting you to integrate with KDE and GNOME. This issue occurs
> with the first-time initialization of gstreamer-0.10. If you encounter this issue, terminate the installation process (using the keys ctrl+c) and run the command
> `gst-inspect-0.10 -- gst-disable-registry-fork --version`. After running the command, you can rerun the tarball package without experiencing the issue.
>
{style="note"}

5. After the installation is complete, you can start the Citrix Workspace app from the application menu.

### How to uninstall Citrix Workspace app

> **Important**
>
> To uninstall Citrix Workspace app, you must be logged in as the same user who does the installation. When you uninstall the Citrix Workspace app, out-of-date cache files at
> `$HOME/.local/share/webkitgtk` might not be removed automatically. As a workaround, manually remove the cache files.
>
> ```Bash
> rm -rf $HOME/.local/share/webkitgtk
> ```
>
{style="note"}

<br/>

To uninstall the Citrix Workspace app, run the following command:

```Bash
cd ~/.tmp
sudo ./setupwfc
```

When the setup program starts, type 2 and press Enter to remove the client.

## JetBrains Toolbox

```Bash
sudo pamac install jetbrains-toolbox
```

## Waydroid {collapsible="true"}

Install Waydroid from the AUR repository:

```Bash
sudo pamac install waydroid
```

Once the installation is completed, you need to make sure the waydroid-container service has started:

```Bash
systemctl status waydroid-container
```

You can enable it to start at boot time:

```Bash
sudo systemctl enable --now waydroid-container
```

Then launch Waydroid from the application menu and follow the first-launch wizard.

If prompted, use the following links for System OTA and Vendor OTA:

```Generic
https://ota.waydro.id/system
https://ota.waydro.id/vendor
```

Install Google apps:

```Bash
sudo waydroid init -s GAPPS
```

The network should work out of the box. If it's not working, you might need to make sure packet forwarding is enabled in kernel and allow the following rules through your firewall
before running Waydroid session start.

Taking ufw as an example:

- DNS traffic needs to be allowed:

```Generic
sudo ufw allow 67
sudo ufw allow 53
```

- Packet forwarding needs to be allowed:

```Generic
sudo ufw default allow FORWARD
```

Make the following changes to the Waydroid configuration file:

```Bash
sudo vim /var/lib/waydroid/waydroid_base.prop
```

```Generic
ro.hardware.gralloc=default
ro.hardware.egl=swiftshader
```

Then restart the Waydroid container service:

```Bash
sudo systemctl restart waydroid-container
```

### How to start Waydroid from the console

```Bash
waydroid session start
waydroid show-full-ui
```

### How to install an APK from the console

Launch the Waydroid shell:

```Bash
sudo waydroid shell
```

```Bash
waydroid app install `/path/to/apk`
```

Get a list of installed applications:

```Bash
waydroid app list
```

Launch an app:

```Bash
waydroid app launch <package-name>
```

### How to stop Waydroid from the console:

```Bash
waydroid session stop;sudo waydroid container stop
```

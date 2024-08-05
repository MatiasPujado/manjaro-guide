# Flatpak Apps

Flatpak is a utility for software deployment and package management for Linux. It is advertised as offering a sandbox environment in which users can run application software in
isolation from the rest of the system.

A Flatpak app can be installed **system-wide** in `/var/lib/flatpak/app`. There is another option, you can also install them in the home directory `$HOME/.local/share/flatpak/app`,
and it will only be visible *user-account-wide**.

## Flatpak Admin Apps

### Warehouse

Warehouse is an app store for Flatpak apps. It is a graphical app that allows you to search for, install, and manage Flatpak apps.

```Bash
flatpak install flathub io.github.flattool.Warehouse
```

### Flatseal

Flatseal is a utility for managing permissions for Flatpak apps. It allows you to view and modify the permissions of installed Flatpak apps.

```Bash
flatpak install flathub com.github.tchx84.Flatseal
```

## Bottles:

Bottles is a utility for managing Wine and Windows apps. It allows you to create and manage Wine prefixes, install Windows apps, and run them in a sandboxed environment.

```Bash
flatpak install flathub com.usebottles.bottles
```

## Telegram

Telegram is a popular messaging app that is available as a Flatpak app.

```Bash
flatpak install flathub org.telegram.desktop
```

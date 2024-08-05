# Gnome Extensions

## List installed extensions

Your locally installed gnome Shell extensions, i.e., these that are installed for your user only, can be listed with the command:

```Bash
ls -al ~/.local/share/gnome-shell/extensions/
```

System-wide installed gnome-shell extensions are listed with the command:

```Bash
ls -al /usr/share/gnome-shell/extensions/
```

You can find out which extensions are enables by querying a dconf setting:

```Bash
gsettings get org.gnome.shell enabled-extensions
```

## GSConnect

If you are using a firewall, you need to open port 1714 to 1764.

```Bash
sudo ufw allow 1714:1764/udp
sudo ufw allow 1714:1764/tcp
sudo ufw reload
```


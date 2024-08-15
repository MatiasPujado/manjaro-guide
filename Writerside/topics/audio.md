# Audio

## Pipewire & Wireplumber

**PipeWire** is a new low-level multimedia framework. It aims to offer capture and playback for both audio and video with minimal latency and support for PulseAudio, JACK, ALSA,
and GStreamer-based applications.

The daemon based on the framework can be configured to be both an audio server (with PulseAudio and JACK features) and a video capture server.

PipeWire also supports containers like Flatpak and does not rely on the audio and video user groups. Instead, it uses a Polkit-like security model, asking Flatpak or Wayland for
permission to record screen or audio.

**WirePlumber** is a powerful session and policy manager for PipeWire. Based on a modular design, with Lua plugins that implement the actual management functionality, it is highly
configurable and extendable.

<br/>

```Bash
sudo pacman -S --needed manjaro-pipewire pipewire-jack pipewire-v4l2 pipewire-docs wireplumber
```

Check if PipeWire is running:

```Bash
LANG=C pactl info | grep '^Server Name'
```

It should return a similar message to this:

```Bash
❯ LANG=C pactl info | grep '^Server Name'
Server Name: PulseAudio (on PipeWire 1.2.1)
```

Run speaker test:

```Bash
speaker-test -c2 -l3 -twav
```

## Codecs and multimedia dependencies

```Bash
sudo pacman -S --needed gstreamer gstreamer-docs gstreamer-vaapi ffmpegthumbnailer vorbis-tools gst-libav ffmpeg gst-plugins-bad gst-plugins-base gst-plugins-good gst-plugins-ugly libde265 openh264 x264 x265 ffmpeg4.4 ffmpegthumbs giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libxinerama libgcrypt lib32-libgcrypt ncurses lib32-ncurses ocl-icd lib32-ocl-icd libxslt lib32-xslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader mc vifm aspell cdrtools glew lib32-dbus-glib lib32-freeglut lib32-glew lib32-gtk2 lib32-imlib2 lib32-libappindicator-gtk2 lib32-libcaca lib32-libcurl-compat lib32-libcurl-gnutls lib32-libdbusmenu-glib lib32-libdbusmenu-gtk2 lib32-libid3tag lib32-libidn11 lib32-libindicator-gtk2 lib32-libjpeg6-turbo lib32-libmikmod lib32-libmodplug lib32-libnm lib32-libpng12 lib32-librtmp0 lib32-libtheora lib32-libtiff lib32-libudev0-shim lib32-libvpx lib32-libwebp lib32-openssl lib32-pipewire lib32-sdl lib32-sdl2_image lib32-sdl2_mixer lib32-sdl2_ttf lib32-sdl_image lib32-sdl_mixer lib32-sdl_ttf lib32-smpeg libcurl-compat libcurl-gnutls libdbusmenu-gtk2 libgcrypt15 libidn11 libindicator-gtk2 libjpeg6-turbo librtmp0 libtiff4 libudev0-shim libvpx openssl opusfile sdl2_image sdl2_mixer sdl2_ttf sdl_image sdl_mixer sdl_ttf smpeg
```

## Enable auto-connect

`module-switch-on-connect` is a PulseAudio module that automatically switches the audio output to a newly connected device. To enable it, run:

```Bash
pactl load-module module-switch-on-connect
```

## Multimedia Stream Managers

### Helvum

**Helvum** is a GTK-based multimedia stream manager for PipeWire. It allows you to manage streams, devices and nodes, and it can be used to monitor and control the audio and video
streams on your system.

**Helvum** is available on Flathub:

```Bash
flatpak install flathub org.pipewire.Helvum
```

Run it:

```Bash
flatpak run org.pipewire.Helvum
```

### Qpwgraph

**Qpwgraph** is a Qt-based graphical tool for PipeWire. It allows you to monitor and control the audio and video streams on your system.

**Qpwgraph** is available on Flathub:

```Bash
flatpak install flathub org.rncbc.qpwgraph
```

Run it:

```Bash
flatpak run org.rncbc.qpwgraph
```

### Coppwr

**Coppwr** is a GTK-based graphical tool for PipeWire. It allows you to monitor and control the audio and video streams on your system.

**Coppwr** is available on Flathub:

```Bash
flatpak install flathub io.github.dimtpap.coppwr
```

Run it:

```Bash

flatpak run io.github.dimtpap.coppwr
```

### Pwvucontrol

**Pwvucontrol** is a GTK-based graphical tool for PipeWire. It allows you to monitor and control the audio and video streams on your system.

**Pwvucontrol** is available on Flathub:

```Bash
flatpak install flathub com.saivert.pwvucontrol
```

Run it:

```Bash
flatpak run com.saivert.pwvucontrol
```

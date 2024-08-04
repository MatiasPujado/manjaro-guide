# FAQ



ESync - Helps Game Overhead

Check to see if esync is enabled already (most distributions do by default)

ulimit -Hn

If this returns more than 500,000 than ESYNC IS ENABLED! If not, proceed with these instructions:
Change the following files and add this line to the bottom

$ sudo gedit /etc/systemd/system.conf
DefaultLimitNOFILE=524288
$ sudo gedit /etc/systemd/system.conf
DefaultLimitNOFILE=524288
$ sudo gedit /etc/security/limits.conf
username hard nofile 524288  # Note: Change username to your username!!!

-----------------------------------------------------------

Can't enable Steam Proton manager

Starting from version 2022.6.28, Bottles allows you to manage Steam Proton prefixes by enabling the setting of the same name.
If you have installed Bottles via Flatpak, it is essential to give it permissions to access the Steam path, otherwise the integration will not be activated.
Proceed according to your Steam installation.

Steam non-Flatpak
flatpak override --user com.usebottles.bottles --filesystem=xdg-data/Steam


-------------------------------------------------------



Spotify

sudo pacman -S --needed spotify-launcher zenity ffmpeg ffmpeg4.4 ffmpegthumbs

# gpg key epiring on 2023-01-20 was exchanged
sudo mv /usr/share/spotify-launcher/keyring.pgp  /usr/share/spotify-launcher/keyring.pgp.old
sudo wget https://download.spotify.com/debian/pubkey_7A3A762FAFD4A51F.gpg -O /usr/share/spotify-launcher/keyring.pgp

----------------------------------------------------------



#############################################################################
Fix Pipewire crackling sound
#############################################################################
# Copy config files to apply account-wide configurations
mkdir -p ~/.config/wireplumber && cp /usr/share/wireplumber/*.conf ~/.config/wireplumber
mkdir -p ~/.config/pipewire && cp /usr/share/pipewire/*.conf ~/.config/pipewire

# Copy config files to apply system-wide configurations
sudo mkdir -p /etc/wireplumber && sudo cp /usr/share/wireplumber/*.conf /etc/wireplumber
sudo mkdir -p /etc/pipewire && sudo cp /usr/share/pipewire/*.conf /etc/pipewire






# Fix auto-switching
rm -rf ~/.config/pulse




# Add the following line
default.clock.allowed-rates = [ 44100 48000 ]

# Daemon config file for PipeWire version "1.2.1" #
#
# Copy and edit this file in /etc/pipewire for system-wide changes
# or in ~/.config/pipewire for local changes.
#
# It is also possible to place a file with an updated section in
# /etc/pipewire/pipewire.conf.d/ for system-wide changes or in
# ~/.config/pipewire/pipewire.conf.d/ for local changes.
#

context.properties = {
## Configure properties in the system.
#library.name.system                   = support/libspa-support
#context.data-loop.library.name.system = support/libspa-support
#support.dbus                          = true
#link.max-buffers                      = 64
link.max-buffers                       = 16                       # version < 3 clients can't handle more
#mem.warn-mlock                        = false
#mem.allow-mlock                       = true
#mem.mlock-all                         = false
#clock.power-of-two-quantum            = true
#log.level                             = 2
#cpu.zero.denormals                    = false

    #loop.rt-prio = -1            # -1 = use module-rt prio, 0 disable rt
    #loop.class = data.rt
    #thread.affinity = [ 0 1 ]    # optional array of CPUs
    #context.num-data-loops = 1   # -1 = num-cpus, 0 = no data loops
    #
    #context.data-loops = [
    #    {   loop.rt-prio = -1
    #        loop.class = [ data.rt audio.rt ]
    #        #library.name.system = support/libspa-support
    #        thread.name = data-loop.0
    #        #thread.affinity = [ 0 1 ]    # optional array of CPUs
    #    }
    #]

    core.daemon = true              # listening for socket connections
    core.name   = pipewire-0        # core name and socket name

    ## Properties for the DSP configuration.
    #default.clock.rate          = 48000
    default.clock.rate          = 48000
    #default.clock.allowed-rates = [ 48000 ]
    default.clock.allowed-rates = [ 32000 44100 48000 88200 96000 192000 ]
    #default.clock.quantum       = 1024
    #default.clock.min-quantum   = 32
    default.clock.min-quantum   = 16
    #default.clock.max-quantum   = 2048
    #default.clock.quantum-limit = 8192
    #default.clock.quantum-floor = 4
    #default.video.width         = 640
    #default.video.height        = 480
    #default.video.rate.num      = 25
    #default.video.rate.denom    = 1
    #
    #settings.check-quantum      = false
    #settings.check-rate         = false

    # keys checked below to disable module loading
    module.x11.bell = true
    # enables autoloading of access module, when disabled an alternative
    # access module needs to be loaded.
    module.access = true
    # enables autoloading of module-jackdbus-detect
    module.jackdbus-detect = true
}

context.properties.rules = [
{   matches = [ { cpu.vm.name = !null } ]
actions = {
update-props = {
# These overrides are only applied when running in a vm.
default.clock.min-quantum = 1024
}
}
}
]

context.spa-libs = {
#<factory-name regex> = <library-name>
#
# Used to find spa factory names. It maps an spa factory name
# regular expression to a library name that should contain
# that factory.
#
audio.convert.* = audioconvert/libspa-audioconvert
avb.*           = avb/libspa-avb
api.alsa.*      = alsa/libspa-alsa
api.v4l2.*      = v4l2/libspa-v4l2
api.libcamera.* = libcamera/libspa-libcamera
api.bluez5.*    = bluez5/libspa-bluez5
api.vulkan.*    = vulkan/libspa-vulkan
api.jack.*      = jack/libspa-jack
support.*       = support/libspa-support
video.convert.* = videoconvert/libspa-videoconvert
#videotestsrc   = videotestsrc/libspa-videotestsrc
#audiotestsrc   = audiotestsrc/libspa-audiotestsrc
}

context.modules = [
#{ name = <module-name>
#    ( args  = { <key> = <value> ... } )
#    ( flags = [ ( ifexists ) ( nofail ) ] )
#    ( condition = [ { <key> = <value> ... } ... ] )
#}
#
# Loads a module with the given parameters.
# If ifexists is given, the module is ignored when it is not found.
# If nofail is given, module initialization failures are ignored.
# If condition is given, the module is loaded only when the context
# properties all match the match rules.
#

    # Uses realtime scheduling to boost the audio thread priorities. This uses
    # RTKit if the user doesn't have permission to use regular realtime
    # scheduling. You can also clamp utilisation values to improve scheduling
    # on embedded and heterogeneous systems, e.g. Arm big.LITTLE devices.
    { name = libpipewire-module-rt
        args = {
            nice.level    = -11
            rt.prio       = 88
            #rt.time.soft = -1
            #rt.time.hard = -1
            #uclamp.min = 0
            #uclamp.max = 1024
        }
        flags = [ ifexists nofail ]
    }

    # The native communication protocol.
    { name = libpipewire-module-protocol-native
        args = {
            # List of server Unix sockets, and optionally permissions
            #sockets = [ { name = "pipewire-0" }, { name = "pipewire-0-manager" } ]
        }
    }

    # The profile module. Allows application to access profiler
    # and performance data. It provides an interface that is used
    # by pw-top and pw-profiler.
    { name = libpipewire-module-profiler }

    # Allows applications to create metadata objects. It creates
    # a factory for Metadata objects.
    { name = libpipewire-module-metadata }

    # Creates a factory for making devices that run in the
    # context of the PipeWire server.
    { name = libpipewire-module-spa-device-factory }

    # Creates a factory for making nodes that run in the
    # context of the PipeWire server.
    { name = libpipewire-module-spa-node-factory }

    # Allows creating nodes that run in the context of the
    # client. Is used by all clients that want to provide
    # data to PipeWire.
    { name = libpipewire-module-client-node }

    # Allows creating devices that run in the context of the
    # client. Is used by the session manager.
    { name = libpipewire-module-client-device }

    # The portal module monitors the PID of the portal process
    # and tags connections with the same PID as portal
    # connections.
    { name = libpipewire-module-portal
        flags = [ ifexists nofail ]
    }

    # The access module can perform access checks and block
    # new clients.
    { name = libpipewire-module-access
        args = {
            # Socket-specific access permissions
            #access.socket = { pipewire-0 = "default", pipewire-0-manager = "unrestricted" }

            # Deprecated legacy mode (not socket-based),
            # for now enabled by default if access.socket is not specified
            #access.legacy = true
        }
        condition = [ { module.access = true } ]
    }

    # Makes a factory for wrapping nodes in an adapter with a
    # converter and resampler.
    { name = libpipewire-module-adapter }

    # Makes a factory for creating links between ports.
    { name = libpipewire-module-link-factory }

    # Provides factories to make session manager objects.
    { name = libpipewire-module-session-manager }

    # Use libcanberra to play X11 Bell
    { name = libpipewire-module-x11-bell
        args = {
            #sink.name = "@DEFAULT_SINK@"
            #sample.name = "bell-window-system"
            #x11.display = null
            #x11.xauthority = null
        }
        flags = [ ifexists nofail ]
        condition = [ { module.x11.bell = true } ]
    }
    { name = libpipewire-module-jackdbus-detect
        args = {
            #jack.library     = libjack.so.0
            #jack.server      = null
            #jack.client-name = PipeWire
            #jack.connect     = true
            #tunnel.mode      = duplex  # source|sink|duplex
            source.props = {
                #audio.channels = 2
		#midi.ports = 1
                #audio.position = [ FL FR ]
                # extra sink properties
            }
            sink.props = {
                #audio.channels = 2
		#midi.ports = 1
                #audio.position = [ FL FR ]
                # extra sink properties
            }
        }
        flags = [ ifexists nofail ]
        condition = [ { module.jackdbus-detect = true } ]
    }
]

context.objects = [
#{ factory = <factory-name>
#    ( args  = { <key> = <value> ... } )
#    ( flags = [ ( nofail ) ] )
#    ( condition = [ { <key> = <value> ... } ... ] )
#}
#
# Creates an object from a PipeWire factory with the given parameters.
# If nofail is given, errors are ignored (and no object is created).
# If condition is given, the object is created only when the context properties
# all match the match rules.
#
#{ factory = spa-node-factory   args = { factory.name = videotestsrc node.name = videotestsrc node.description = videotestsrc "Spa:Pod:Object:Param:Props:patternType" = 1 } }
#{ factory = spa-device-factory args = { factory.name = api.jack.device foo=bar } flags = [ nofail ] }
#{ factory = spa-device-factory args = { factory.name = api.alsa.enum.udev } }
#{ factory = spa-node-factory   args = { factory.name = api.alsa.seq.bridge node.name = Internal-MIDI-Bridge } }
#{ factory = adapter            args = { factory.name = audiotestsrc node.name = my-test node.description = audiotestsrc } }
#{ factory = spa-node-factory   args = { factory.name = api.vulkan.compute.source node.name = my-compute-source } }

    # A default dummy driver. This handles nodes marked with the "node.always-process"
    # property when no other driver is currently active. JACK clients need this.
    { factory = spa-node-factory
        args = {
            factory.name    = support.node.driver
            node.name       = Dummy-Driver
            node.group      = pipewire.dummy
            node.sync-group  = sync.dummy
            priority.driver = 20000
            #clock.id       = monotonic # realtime | tai | monotonic-raw | boottime
            #clock.name     = "clock.system.monotonic"
        }
    }
    { factory = spa-node-factory
        args = {
            factory.name    = support.node.driver
            node.name       = Freewheel-Driver
            priority.driver = 19000
            node.group      = pipewire.freewheel
            node.sync-group  = sync.dummy
            node.freewheel  = true
            #freewheel.wait = 10
        }
    }

    # This creates a new Source node. It will have input ports
    # that you can link, to provide audio for this source.
    #{ factory = adapter
    #    args = {
    #        factory.name     = support.null-audio-sink
    #        node.name        = "my-mic"
    #        node.description = "Microphone"
    #        media.class      = "Audio/Source/Virtual"
    #        audio.position   = "FL,FR"
    #        monitor.passthrough = true
    #    }
    #}

    # This creates a single PCM source device for the given
    # alsa device path hw:0. You can change source to sink
    # to make a sink in the same way.
    #{ factory = adapter
    #    args = {
    #        factory.name           = api.alsa.pcm.source
    #        node.name              = "alsa-source"
    #        node.description       = "PCM Source"
    #        media.class            = "Audio/Source"
    #        api.alsa.path          = "hw:0"
    #        api.alsa.period-size   = 1024
    #        api.alsa.headroom      = 0
    #        api.alsa.disable-mmap  = false
    #        api.alsa.disable-batch = false
    #        audio.format           = "S16LE"
    #        audio.rate             = 48000
    #        audio.channels         = 2
    #        audio.position         = "FL,FR"
    #    }
    #}

    # Use the metadata factory to create metadata and some default values.
    #{ factory = metadata
    #    args = {
    #        metadata.name = my-metadata
    #        metadata.values = [
    #            { key = default.audio.sink   value = { name = somesink } }
    #            { key = default.audio.source value = { name = somesource } }
    #        ]
    #    }
    #}
]

context.exec = [
#{   path = <program-name>
#    ( args = "<arguments>" | [ <arg1> <arg2> ... ] )
#    ( condition = [ { <key> = <value> ... } ... ] )
#}
#
# Execute the given program with arguments.
# If condition is given, the program is executed only when the context
# properties all match the match rules.
#
# You can optionally start the session manager here,
# but it is better to start it as a systemd service.
# Run the session manager with -h for options.
#
#{ path = "/usr/bin/pipewire-media-session" args = ""
#  condition = [ { exec.session-manager = null } { exec.session-manager = true } ] }
#
# You can optionally start the pulseaudio-server here as well
# but it is better to start it as a systemd service.
# It can be interesting to start another daemon here that listens
# on another address with the -a option (eg. -a tcp:4713).
#
#{ path = "/usr/bin/pipewire" args = [ "-c" "pipewire-pulse.conf" ]
#  condition = [ { exec.pipewire-pulse = null } { exec.pipewire-pulse = true } ] }
]

#############################################################################



#############################################################################
Fix allow non-root process to bind to priviliged ports(443,80)
#############################################################################

# setcap CAP_NET_BIND_SERVICE=+eip /usr/bin/ssh

#############################################################################
Fix Firefox
#############################################################################

## To Fix Video Playback

In 'about:config'

'general.useragent.override'

select 'String' and click '+' and add:

'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0'


## To Fix CSV Password File Loading

In 'about:config'

'signon.management.page.fileImport.enabled'  --> set it to 'TRUE' to enable csv file importing

'security.allow_eval_in_parent_process'  --> set it to 'TRUE' to force Firefox in reading the csv file



#############################################################################
Fix Sonarqube
#############################################################################

To query all limits
ulimit -a

sudo vim /etc/sysctl.d/99-sysctl.conf
vm.swappiness=10
vm.max_map_count=262144
fs.file-max=131072


## Ignore ##
sudo vim /etc/security/limits.conf
nofile=131072:131072
############

#############################################################################
Fix Docker: library initialization failed - unable to allocate file
descriptor table - out of memory
#############################################################################

sudo gedit /usr/lib/systemd/system/docker.service

and then enter

[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --default-ulimit nofile=131072:131072 -H fd://

[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --default-ulimit nofile=65536:65536 -H fd://

Save and run sudo systemctl daemon-reload and sudo systemctl restart docker


Fix Keyring

sudo pacman -Syy
sudo pacman-key --refresh-keys
sudo pacman-key --populate archlinux manjaro
sudo pacman -Sc




IMPORT CERTS: https://warlord0blog.wordpress.com/2021/01/17/trusting-ca-certificates-manjaro/

sudo trust anchor --store ~/.ssh/certs/ca-seguridad-informatica.crt

It’s quite clever, it puts it into /etc/ca-certificates and renames it to match the actual name of the certificate.

sudo update-ca-trust

You can still check your certificates are imported using:

awk -v cmd='openssl x509 -noout -subject' '/BEGIN/{close(cmd)};{print | cmd}' < /etc/ssl/certs/ca-certificates.crt

------------------------------------------------------


#############################################################################
#
#						ORACLE Solution
#
#############################################################################

To find a user's UID or GID in Unix, use the id command. To find a specific user's UID, at the Unix prompt, enter:

id -u username

Replace username with the appropriate user's username. To find a user's GID, at the Unix prompt, enter:

id -g username

If you wish to find out all the groups a user belongs to, instead enter:

id -G username

If you wish to see the UID and all groups associated with a user, enter id without any options, as follows:

id username



## Create group with specific ID
sudo groupadd -g 54321 oinstall

## Create user account with specific ID
# Create user account without a Home directory
sudo useradd -Mu 54321 -G oinstall,matias,admin,coder,docker,dev oracle

# Then lock the account to prevent logging in:
sudo usermod -L oracle

-------------------------------------------------------------------

General tips

Waydroid is in rapid development so if you face issues, here is a good list of steps to do first:

    Make sure your Waydroid package is up to date.
    Make sure you have the latest Waydroid image by running

    # waydroid upgrade

    .
    Reset Waydroid: stop the waydroid-container.service, run

    # waydroid init -f

    and start the service again.
    You may also want to do little cleanup, run

    # rm -rf /var/lib/waydroid /home/.waydroid

    $ rm -rf ~/waydroid ~/.share/waydroid ~/.local/share/applications/*aydroid* ~/.local/share/waydroid


##Device isn't play protect certified
https://github.com/waydroid/waydroid/issues/753
https://github.com/casualsnek/waydroid_script/issues/26

https://www.google.com/android/uncertified/

Old way to read android_id direct in terminal of base system:
sudo sqlite3 /var/lib/waydroid/data/data/com.google.android.gsf/databases/gservices.db "select * from main where name='android_id';" | cut -f2 -d"|"

New way:
sudo grep android_id ~/.local/share/waydroid/data/data/com.google.android.gms/shared_prefs/Checkin.xml | cut -f2 -d">" | cut -f1 -d"<"

Maybe in your system need change ~ to your correct /home/user

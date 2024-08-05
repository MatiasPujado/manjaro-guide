# Utilities

## NodeJS {collapsible="true"}

### From the official repository

```Bash
sudo pacman -S --needed nodejs npm
```

### From NVM (Node Version Manager)

To install or update nvm, run one of the following commands:

```Bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```

```Bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```

Check nvm version:

```Bash
nvm --version
```

#### Important Commands

If you want to use the system-installed version of node, you can use the following command:

```Bash
nvm use system
```

To see the version of the system-installed node:

```Bash
nvm run system --version
```

Listing installed versions:

```Bash
nvm ls
```

List available remote versions:

```Bash
nvm ls-remote
```

Install the desired version:

```Bash
nvm install v18
```

Open a new terminal window and set the default version:

```Bash
nvm use v18.18.2
nvm alias default v18.18.2
```

Now you can check the version:

```Bash
node --version
npm --version
```

## Python {collapsible="true"}

Get Python for official repositories:

```Bash
sudo pacman -S --needed python python-virtualenv python-pipenv python-virtualenvwrapper
```

### How to use it

List of virtual environments created:

```Bash
workon
```

Create a virtual environment (all command line options except -a, -i, -r, and -h are passed directly to virtualenv, so you can use -p to select a Python version):

```Bash
mkvirtualenv <environment-name>
```

Activate the virtual environment:

```Bash
workon <environment-name>
```

Install some package inside the virtual environment (say, Django):

```Bash
<environment-name> $ pip install django
```

Once you are done, deactivate the virtual environment:

```Bash
<environment-name> $ deactivate
```

To delete a virtual environment:

```Bash
rm -rf ~/.virtualenvs/<environment-name>
```



## AmdFan {collapsible="true"}

**amdfan** is a fork of **amdgpu-fan**, with security updates and added functionality. This is intended for stand-alone GPU's and not integrated APU's.

### Installation

From the AUR, already configured for Manjaro (might be outdated):

```Bash
yay -S amdfan
```

Install amdfan in a virtual environment and set it up:

```Bash
pip install amdfan
```

### Documentation

```Bash
Usage: amdfan.py [OPTIONS]

Options:
--daemon         Run as daemon applying the fan curve
--monitor        Run as a monitor showing temp and fan speed
--manual         Manually set the fan speed value of a card
--configuration  Prints out the default configuration for you to use
--service        Prints out the amdfan.service file to use with systemd
--help           Show this message and exit.
```

### Configuration

Paste the following content in /etc/amdfan.yml. This allows you to configure the settings before running it as a daemon.

```Bash
sudo gedit /etc/amdfan.yml
```

```Bash
#Fan Control Matrix.
# [<Temp in C>,<Fanspeed in %>]
speed_matrix:
- [4, 4]
- [30, 33]
- [45, 50]
- [60, 66]
- [65, 69]
- [70, 75]
- [75, 89]
- [80, 100]

# Current Min supported value is 4 due to driver bug
#
# Optional configuration options
#
# Allows for some leeway +/- temp, as to not constantly change fan speed
# threshold: 4
#
# Frequency will change how often we probe for the temp
# frequency: 5
#
# While frequency and threshold are optional, I highly recommend finding
# settings that work for you. I've included the defaults I use above.
#
# cards:
# can be any card returned from `ls /sys/class/drm | grep "^card[[:digit:]]$"`
# - card0
```

Now we set up the systemd service:

```Bash
sudo vim  /etc/systemd/system/amdfan.service
```

```Bash
[Unit]
Description=amdfan controller
After=multi-user.target
Requires=multi-user.target

[Service]
ExecStart=/usr/bin/amdfan daemon
Restart=always

[Install]
WantedBy=final.target
```

Confirm that it has been installed in `$HOME/.virtualenvs/amdfan/bin` and not in `/usr/bin` with the following command:

```Bash
which amdfan
```

If it is in '/usr/lib/bin', create a symbolic link to '/usr/bin':

```Bash
sudo ln -s $HOME/.virtualenvs/amdfan/bin/amdfan /usr/bin/amdfan
```

Now we enable and start the service:

```Bash
sudo systemctl enable --now amdfan
```

### Updating

If we make changes to the configuration file, we need to make systemd aware of the changes:

```Bash
sudo systemctl daemon-reload
```

Then we need to restart the service:

```Bash
sudo systemctl restart amdfan
```

To check the status of the service:

```Bash
systemctl status amdfan
```

## HeadsetControl {collapsible="true"}

### How to install

If you skipped the step [Install important dependencies](first-steps.md#install-important-dependencies), go back and get them. Some are essential for the installation of
HeadsetControl.

```Bash
cd ~/.tmp
```

```Bash
git clone https://github.com/Sapd/HeadsetControl && cd HeadsetControl
```

```Bash
mkdir -p build && cd build
```

```Bash
cmake ..
```

```Bash
make
```

```Bash
sudo make install
```

At this point we can only access the application with root. To fix this, we need a udev rule.

### Access without sudo

On Linux, you need udev rules if you don't want to start the application with root permissions. Those rules are generated via `headsetcontrol -u`. During the last step of the
installation process `make install` generates and writes them automatically to `/etc/udev/rules.d/`.

You can reload udev configuration without a reboot via:

```Bash
sudo udevadm control --reload-rules && sudo udevadm trigger
```

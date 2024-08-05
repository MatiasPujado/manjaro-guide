# Terminal Emulator

## What is a terminal emulator? {collapsible="true"}

A terminal emulator is a program that allows the user to interact with the computer through a command-line interface. It is a software application that provides a text-based
interface for users to input commands and receive output from the system. Terminal emulators are commonly used by developers, system administrators, and power users to perform
tasks that aren’t easily achieved through a graphical user interface.

In Unix terminology, we can understand this as:

- terminal = tty = text input/output environment
- console = physical terminal
- shell = command line interpreter

Console, terminal, and tty are closely related. Originally, they meant a piece of equipment through which you could interact with a computer. In the early days of Unix, that meant
a teleprinter-style device resembling a typewriter, sometimes called a teletypewriter, or “tty” in shorthand. The name “terminal” came from the electronic point of view, and the
name “console” from the furniture point of view. Very early in Unix history, electronic keyboards and displays became the norm for terminals.

In Unix terminology, a tty is a particular kind of device file that implements several additional commands (ioctls) beyond read and write. In its most common meaning, terminal is
synonymous with tty. Some ttys are provided by the kernel on behalf of a hardware device, for example, with the input coming from the keyboard, and the output going to a text mode
screen, or with the input and output transmitted over a serial line. Other ttys, sometimes called pseudo-ttys, are provided (through a thin kernel layer) by programs called
terminal emulators, such as Xterm (running in the X Window System), Screen (which provides a layer of isolation between a program and another terminal), SSH (which connects a
terminal on one machine with programs on another machine), Expect (for scripting terminal interactions), etc.

The word **terminal** can also have a more traditional meaning of a device through which one interacts with a computer, typically with a keyboard and a display. For example, an X
terminal is a kind of thin client, a special-purpose computer whose only purpose is to drive a keyboard, display, mouse, and occasionally other human interaction peripherals, with
the actual applications running on another, more powerful computer.

A **console** is generally a terminal in the physical sense that is, by some definition, the primary terminal directly connected to a machine. The console appears to the operating
system as a (kernel-implemented) tty. On some systems, such as Linux and FreeBSD, the console appears as several ttys (special key combinations switch between these ttys); just to
confuse matters, the name given to each particular tty can be “console”, “virtual console”, “virtual terminal”, and other variations.

A **shell** is the primary interface that users see when they log in, whose primary purpose is to start other programs. In Unix circles, shell has specialized to mean a
command-line shell, centered around entering the name of the application one wants to start, followed by the names of files or other objects that the application should act on, and
pressing the Enter key. Other types of environments don't use the word “shell”; for example, window systems involve “window managers” and “desktop environments”, not a “shell”.

There are many different Unix shells. Popular shells for interactive use include Bash (the default on most Linux installations), Zsh (which emphasizes power and customizability)
and fish (which emphasizes simplicity).

Command-line shells include flow control constructs to combine commands. Besides typing commands at an interactive prompt, users can write scripts. The most common shells have a
common syntax based on the Bourne shell. When discussing “shell programming”, the shell is almost always implied to be a Bourne-style shell. Some shells that are often used for
scripting but lack advanced interactive features such as the KornShell (ksh), and many ash variants. Pretty much any Unix-like system has a Bourne-style shell installed as /bin/sh,
usually ash, ksh, or Bash.

In Unix system administration, a user's shell is the program invoked when they log in. Normal user accounts have a command-line shell, but users with restricted access may have a
restricted shell, or some other specific command (e.g. for file-transfer-only accounts).

The division of labor between the terminal, and the shell is not completely obvious. Here are their main tasks:

- Input: the **terminal** converts keys into control sequences (e.g. Left → \e[D). The **shell** converts control sequences into commands (e.g. \e[D → backward-char).
- The **shell** provides Line editing, input history, and completion. The **terminal** may provide its own line editing, history, and completion and only send a line to the shell
  when it's ready to be executed. The only common terminals that operate in this way are M-x shell in Emacs.
- Output: the **shell** emits instructions such as “display foo”, “switch the foreground color to green”, “move the cursor to the next line”, etc. The **terminal** acts on these
  instructions.
- The prompt is purely a shell concept.
- The **shell** never sees the output of the commands it runs (unless redirected). Output history (scrollback) is purely a **terminal** concept.
- The **terminal** provides inter-application copy-paste (usually with the mouse or key sequences such as Ctrl+Shift+V or Shift+Insert). The shell may have its own internal
  copy-paste mechanism as well (e.g. Meta+W and Ctrl+Y).
- The **shell** mostly performs Job control (launching programs in the background and managing them). However, it's the **terminal** that handles key combinations like Ctrl+C to
  kill the foreground job and Ctrl+Z to suspend it.

[Source](https://unix.stackexchange.com/questions/4126/what-is-the-exact-difference-between-a-terminal-a-shell-a-tty-and-a-con)

## Shell ZSH

I'm going to be using zsh as my shell. Zsh is a shell designed for interactive use, although it is also a powerful scripting language. A lot of useful features from bash and ksh
were incorporated into zsh; many original features were added. The goal is to increase the efficiency and ease of use for the CLI user.

Install ZSH and some additional packages:

```Bash
sudo pacman -S --needed zsh zsh-doc
```

Install open source fonts:

```Bash
sudo pacman -S --needed gnu-free-fonts ttf-carlito ttf-croscore ttf-dejavu ttf-droid ttf-fira-code ttf-fira-mono ttf-font-awesome ttf-ibm-plex ttf-hack ttf-joypixels ttf-roboto ttf-ubuntu-font-family ttf-dejavu ttf-terminus-nerd ttf-input awesome-terminal-fonts nerd-fonts
```

Install proprietary fonts:

```Bash
yay -S ttf-ms-fonts
```

Source font recently installed:

```Bash
fc-cache -f -v
```

List all installed shells, run:

```Bash
chsh -l
```

Set one as default for your user do:

```Bash
chsh -s $(which zsh)
```

If you now log out and log in again, you will be greeted by the other shell; or just open a new terminal window.

> **Note**
>
> 'chsh' uses /etc/shells as reference. If a recently installed shell is not present on the list, it can be manually added to this file.
>
{style="note"}

<br/>

Different ways to check you are using zsh:

```Bash
echo $SHELL
```

```Bash
echo $0
```

```Bash
ps -p $$
```

<br/>

Provide an initial configuration for zsh:

```Bash
autoload -Uz zsh-newuser-install; zsh-newuser-install -f
```

To handle better the installation of certain apps I'm going to create a temp directory:

```Bash
mkdir -p ~/.tmp
```

### Zsh Plugin Manager: Oh-my-Zsh

Install Oh-my-Zsh:

```Bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### Zsh Plugins

- LSD (LSDeluxe)

```Bash
sudo pacman -S --needed lsd
```

- Zsh-autosuggestions

```Bash
sudo pacman -S --needed zsh-autosuggestions
```

- zsh-syntax-highlighting

```Bash
sudo pacman -S --needed zsh-syntax-highlighting
```

- Fuzzy Finder

```Bash
sudo pacman -S fzf
```

- Zsh-interactive-cd

Zsh-interactive-cd is integrated in the fzf plugin. All we need to do is source it in the .zshrc file to start using it.

#### Zsh Theme

- PowerLevel 10K

```Bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Enable the theme in the .zshrc file:

```Bash
vim ~/.zshrc
```

Comment the line with the theme and add the following below:

```Bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

#### Source plugins

> **Note**
>
> The more plugins you source, the slower the shell will start. Pick wisely.
>
{style="note"}

```Bash
plugins=(
  git
  gitignore
  fzf
  zsh-autosuggestions
  zsh-interactive-cd
  zsh-syntax-highlighting
  docker
  docker-compose
)
```

### Account-wide configuration {collapsible="true"}

The following environment variables are set in the .zshrc file:

```
###################################################################
#		Env Var
#	Manjaro
export XDG_CONFIG_HOME=$HOME/.config
export PATH=$PATH:$XDG_CONFIG_HOME

#	Python
export WORKON_HOME="$HOME/.virtualenvs"
export VIRTUALENVWRAPPER_VIRTUALENV=/usr/sbin/virtualenv
export PIP_RESPECT_VIRTUALENV=true
export VIRTUALENVWRAPPER_PYTHON=$(which python3)
export VIRTUALENVWRAPPER_HOOK_DIR="$WORKON_HOME/hooks"
source /usr/bin/virtualenvwrapper_lazy.sh

#	NVM
export NVM_DIR="$HOME/.config/nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
export NVM_COLORS='cmgRY'

#	NPM
NPM_PACKAGES="${HOME}/.npm-packages"
export PATH=$PATH:$NPM_PACKAGES:"$NPM_PACKAGES/bin"
# Preserve MANPATH if you already defined it somewhere in your config.
# Otherwise, fall back to `manpath` so we can inherit from `/etc/manpath`.
export MANPATH=${MANPATH-$(manpath)}:"$NPM_PACKAGES/share/man"
# export npm_config_prefix="$HOME/.local"

#	Jetbrains
export PATH=$PATH:"$HOME/.local/share/JetBrains/Toolbox/scripts"

#	OBS-Studio
export QT_QPA_PLATFORM=xcb,wayland

#	SONARQUBE & POSTGRES
export SONARQUBE_USERNAME=sonar
export SONARQUBE_PASSWORD=sonar
export POSTGRES_USERNAME=sonar
export POSTGRES_PASSWORD=sonar

#	VSCODE
export PATH=$PATH:$HOME/.console-ninja/.bin

#	WINE
# export WINEARCH=win32
# export WINEPREFIX=~/.wine32

# Ruby - Jekyll
export PATH=$PATH:/home/matias/.local/share/gem/ruby/3.0.0/bin

###################################################################
```

### System-wide configuration {collapsible="true"}

The following is a list of useful aliases added to /etc/bash.bashrc and /etc/zsh/zshrc:

```Generic
#################################################################################################
#
# System-wide Aliases for Manjaro
#

##########################
##       System         ##
##########################

### Administration
alias sudo='sudo '
alias upgrade='pacman -Syu'
alias install='pacman -S'
alias search='pacman -Ss'
alias uninstall='pacman -Rs'
alias list-i='pacman -Qe'
alias owner='pacman -Qo'
alias list-orphans='pacman -Qdt'
alias remove-orphans='pacman -Rns $(pacman -Qdtq)'
alias list-packages-in-group='pacman -Sg'
alias list-dep='pactree'
alias md='mkdir -p'
alias ..='cd ..'
alias ...='cd ../..'
alias rm='rm -v'
alias mv='mv -iv'
alias ll='ls -lAh'
alias lla='ls -lah'
alias cl='clear'
alias nowtime='date +"%T"'  # Shows the current time in 24hrs format as HH:MM:SS
#alias nowdate='date +"%d - %m - %Y"'  # Shows the current date in format dd-MM-YY
alias usage='du -hd 1 . | sort -hr'  # Shows how much space occupies each file and folder
alias lsof='lsof -i'  # Shows the list of programs users and files running
alias boot-time='systemd-analyze'
alias boot-time-blame='systemd-analyze blame'
alias list-reboot='last reboot | less'
alias list-shutdown='last shutdown | less'
alias list-ps='ps -aux'  # Lists all processes and executables running that belong to current user

### Important Directories

alias home='cd $HOME'
alias opt='cd /opt'
alias media='cd /media'
alias vault='cd /media/Vault'
alias work='cd /media/Work/Projects/Work'
alias personal='cd /media/Work/Projects/Personal'
alias bbva='cd /media/Work/Projects/Work/bbva'
alias prisma='cd /media/Work/Projects/Work/prisma'
alias games='cd /media/Games'
alias media-center='cd /media/MediaCenter'
# Shared documents in LAN
alias dell="mount -t cifs -o credentials=/etc/win-credentials,uid=1001,gid=1001,dir_mode=0774,file_mode=0774 '\\192.168.100.36\SharedFolder' /media/Dell"
alias mount-dell='mount /media/Dell'
alias umount-dell='umount /media/Dell'
alias umount-lazy-dell='umount -l /media/Dell'

## AMDFan
alias amdfan-active='sudo systemctl is-active amdfan'
alias amdfan-status='sudo systemctl status amdfan'
alias amdfan-restart='sudo systemctl restart amdfan'
alias amdfan-monitor='amdfan --monitor'
alias amdfan-daemon='amdfan --daemon'
alias amdfan-config='amdfan --configuration'
alias amdfan-service='amdfan --service'

## PlexMediaServer
alias plex-active='sudo systemctl is-active plexmediaserver'
alias plex-status='sudo systemctl status plexmediaserver'
alias plex-restart='sudo systemctl restart plexmediaserver'

## Gnome
alias gdm-active='sudo systemctl is-active gdm'
alias gdm-status='sudo systemctl status gdm'
alias gdm-restart='sudo systemctl restart gdm'
alias gdm-reconfig='sudo dpkg-reconfigure gdm'

## Networking
alias net-active='sudo systemctl is-active net'
alias net-status='sudo systemctl status net'
alias net-restart='sudo systemctl restart net'

## Pipewire
alias pw-active='systemctl --user is-active pipewire.service'
alias pw-status='systemctl --user status pipewire.service'
alias pw-restart='systemctl --user restart pipewire.service'

## Wireplumber
alias wp-active='systemctl --user is-active wireplumber.service'
alias wp-status='systemctl --user status wireplumber.service'
alias wp-restart='systemctl --user restart wireplumber.service'

## Docker
alias d-active='systemctl --user is-active docker'
alias d-status='systemctl --user status docker'
alias d-restart='systemctl --user restart docker'

### Network
## Networking
alias my-ip='curl ipinfo.io/ip'
alias ping-google="echo 'Pinging Google' && ping www.google.com"
alias flushdns='sudo systemctl restart dnsmasq'
alias net-interface='ip a'
alias ip-all='nmcli -p device show'

##########################
##         Apps         ##
##########################

### Desktop Apps
## Conky
alias c-reload='killall -SIGUSR1 conky'  # Realoads Conky without having to kill the process
alias c-kill='killall conky'  # Kills all Conky processes
alias c-test='conky --daemonize --config=$HOME/.conkyrc --pause=5 & disown'  # Starts Conky using .conkyrc from $HOME

## USBView
alias usbview='sudo usbview & disown'

## Weather (wttr.in)
alias w-short='curl -s "wttr.in/Mendoza?format=%l:+%c+%t+(%f)"'
alias w-full='curl -s "wttr.in/Mendoza?format=%l:+%c+%t+(%f)\n+Rain:%p+Wind:%w\n+Pressure:%P+Humidity:%h\n+%m+Day:%M"'
alias w-moon='curl -s "wttr.in/Mendoza?format=%l:+%m+Day:%M"'
alias w-forecast='curl "wttr.in/Mendoza"'
alias w-time='curl -s "wttr.in/Mendoza?format=%l:\n+Dawn:%D\n+Sunrise:%S\n+Zenith:%z\n+Sunset:%s\n+Dusk:%d"'

#################################################################################################
```

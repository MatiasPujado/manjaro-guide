# Dev Tools

## Java {collapsible="true"}

### Installation

The latest OpenJDK

```Bash
sudo pacman -S --needed jdk-openjdk openjdk-doc openjdk-src
```

OpenJDK 17

```Bash
sudo pacman -S --needed jdk17-openjdk openjdk17-doc openjdk17-src
```

OpenJDK 11

```Bash
sudo pacman -S --needed jdk11-openjdk openjdk11-doc openjdk11-src
```

OpenJDK 8

```Bash
sudo pacman -S --needed jre8-openjdk-headless jre8-openjdk jdk8-openjdk openjdk8-doc openjdk8-src
```

VisualVM

```Bash
sudo pacman -S --needed visualvm
```

For other versions, like **Oracle** and **Zulu**, you will have to do the installation manually. Download the desired version and unpack it in `/usr/lib/jvm`:

```Bash
sudo tar -xvzf jdk-17.0.2_linux-x64_bin.tar.gz -C /usr/lib/jvm
```

### Setting the default Java version

The helper script archlinux-java provides such functionalities:

```Bash
archlinux-java <COMMAND>
```

```Bash
COMMAND:
status		List installed Java environments and enabled one
get		Return the short name of the Java environment set as default
set <JAVA_ENV>	Force <JAVA_ENV> as default
unset		Unset current default Java environment
fix		Fix an invalid/broken default Java environment configuration
```

### List compatible Java environments installed

> **Note**
>
> The line with `(default)` is denoting the currently active default version. Invocation of java and other binaries will rely on this Java install.
>
{style="note"}

<br/>

```Bash
archlinux-java status
```

### Changing the default Java version

```Bash
sudo archlinux-java set <java_version_name>
```

### Unsetting the default Java environment

There should be no need to unset a Java environment as packages providing them should take care of this. Still, should you want to do so, just use command unset:

```Bash
sudo archlinux-java unset
```

### Fixing the default Java environment

If an invalid Java environment link is set, calling the archlinux-java fix command tries to fix it. Also note that if no default Java environment is set, this will look for valid
versions and try to set it for you. Officially supported package “OpenJDK 8” will be considered first in this order, then other installed environments.

```Bash
sudo archlinux-java fix
```

## Kotlin {collapsible="true"}

```Bash
sudo pacman -S --needed kotlin ktlint
```

## MySQL - MySQL Workbench {collapsible="true"}

### To install MySQL, choose MariaDB:

```Bash
sudo pacman -S --needed mysql-workbench
```

Verify the installation:

```Bash
mysqld --version
```

### Enable and start the MySQL service:

```Bash
sudo systemctl enable --now mysqld
```

If the following error is prompted:

```Bash
Job for mariadb.service failed because the control process exited with error code.
See "systemctl status mariadb.service" and "journalctl -xeu mariadb.service" for details.
```

Run the following command:

```Bash
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

The output should be similar to this:

```Bash
Installing MariaDB/MySQL system tables in '/var/lib/mysql' ...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system

Two all-privilege accounts were created.
One is root@localhost, it has no password, but you need to
be system 'root' user to connect. Use, for example, sudo mysql
The second is mysql@localhost, it has no password either, but
you need to be the system 'mysql' user to connect.
After connecting you can set the password, if you would need to be
able to connect as any of these users with a password and without sudo

See the MariaDB Knowledgebase at https://mariadb.com/kb

You can start the MariaDB daemon with:
cd '/usr' ; /usr/bin/mysqld_safe --datadir='/var/lib/mysql'

You can test the MariaDB daemon with mysql-test-run.pl
cd '/usr/mysql-test' ; perl mysql-test-run.pl

Please report any problems at https://mariadb.org/jira

The latest information about MariaDB is available at https://mariadb.org/.

Consider joining MariaDB's strong and vibrant community:
https://mariadb.org/get-involved/
```

Now, try [enabling and starting the MySQL service again](dev-tools.md#enable-and-start-the-mysql-service)

Create a soft-link to the required library:

```Bash
sudo ln -s /usr/lib64/libtinfo.so.6 /usr/lib64/libtinfo.so.5
```

### Change root’s predefined password:

```Bash
sudo mysql
```

```Bash
UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
```

Sometimes it can throw an error depending on the mysql version. When that happens, run the following:

```Bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
FLUSH PRIVILEGES;
```

[Source](https://stackoverflow.com/questions/7534056/mysql-root-password-change/53968240#53968240)


### Upgrading MYSQL-MariaDB

Stop the service:

```Bash
sudo systemctl stop mariadb
sudo mysqld_safe --skip-grant-tables --skip-networking &
```

Open a new terminal window and run:

```Bash
sudo mysql -u root
```

```Bash
MariaDB [(none)]> use mysql
```

```Bash
MariaDB [mysql]> flush privileges;
```

```Bash
MariaDB [mysql]> ALTER USER 'root'@'localhost' IDENTIFIED BY 'the_password_i_want';
```

```Bash
MariaDB [mysql]> exit
```

To kill the service, we need to find the PID:

```Bash
ps -aux | grep mysql
```

Then kill the process, run this command for every PID listed:

```Bash
kill -9 <PID>
```

Now, start the service:

```Bash
sudo systemctl start mariadb
```

[Source](https://forum.manjaro.org/t/upgrade-mariadb-error/75052/5)


## PhpMyAdmin {collapsible="true"}

Install the required packages:

```Bash
sudo pacman -S --needed apache php libnotify phpmyadmin
```

Install plug-ins:

```Bash
sudo pacman -S --needed php-apache php-cgi php-fpm php-gd php-embed php-intl php-redis php-snmp
```

## Docker {collapsible="true"}

### Install Docker from the official repositories:

```Bash
sudo pacman -S --needed docker docker-compose docker-scan
```

After installing Docker CE, start and enable the Docker Daemon.

```Bash
sudo systemctl enable --now docker
```

Check the version of the Docker package to confirm it was successfully installed.

```Bash
docker version
```

The output should be like this:

```Bash
Client:
Version:           20.10.22
API version:       1.41
Go version:        go1.19.4
Git commit:        3a2c30b63a
Built:             Tue Dec 20 20:43:40 2022
OS/Arch:           linux/amd64
Context:           default
Experimental:      true
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied
```

### Configuring Docker for Non-Root User

To enable your user account to access and manage docker without administrative privileges, we will add the account to the `docker group`.

```Bash
newgrp docker
```

```Bash
sudo usermod -aG docker $USER
```

Verify that your account is in the `docker group`:

```Bash
id -nG
```

Try running a Docker command without `sudo`:

```Bash
docker run hello-world
```

## Linting {collapsible="true"}

### ESLint

```Bash
sudo pacman -S --needed eslint
```

### Prettier

```Bash
sudo pacman -S --needed prettier prettyping
```

### Stylelint

```Bash
sudo pacman -S --needed stylelint
```

# Version Control

## Git

Git is a distributed version control system widely used for source code management. It is designed to handle everything from small to huge projects with speed and efficiency.

### Git Configuration

The `git config` command is a convenient way to set configuration options that control all aspects of how Git operates. These configuration options are stored in three different
places, each with a different scope:

- **System-level** (--system): These options are applied to every user on the system and all of their repositories. They are stored in the `/etc/gitconfig` file. If you pass the
  `--system` option to git config, it reads and writes from this file specifically.

- **Global-level** (--global): These options are applied to all repositories of the current user. They are stored in the `.gitconfig` or `.config/git/config` file in the user’s
  home directory. If you pass the `--global` option to git config, it reads and writes from this file.

- **Local-level** (--local): These options are applied to the current repository only. They are stored in the `.git/config` file in the repository’s root directory. If you don't
  specify a level when using git config, this is the default scope.

Each level overrides the values from the previous level, so options in a repository's `.git/config` file override those in the user's `.gitconfig` file, which in turn override
those in the `/etc/gitconfig` file.

Here's an example of how to set the username at different levels:

- **System-level:** `sudo git config --system user.name "System User"`
- **Global-level:** `git config --global user.name "Global User"`
- **Local-level:** `git config --local user.name "Local User"`

To see the origin of your configuration, you can use the git `config --list --show-origin` command.

### My Git Configuration

#### System-level Configuration

```Bash
sudo git config --system core.editor vim
sudo git config --system core.excludesFile /etc/gitignore
sudo git config --system init.defaultBranch main
sudo git config --system pull.ff true
sudo git config --system pull.rebase true
sudo git config --system credential.helper /usr/lib/git-core/git-credential-libsecret
sudo git config --system user.name "Matias Pujado"
sudo git config --system user.email matiaspujado@gmail.com
sudo git config --system --add safe.directory '*'
```

### Install GitHub CLI tools

GitHub CLI brings GitHub to your terminal. It enables you to perform many of the same actions you would on GitHub.com, but directly from your terminal.

Act is a tool that runs your GitHub Actions locally. It is a single binary that is straightforward to install and run.

```Bash
sudo pacman -S --needed github-cli
```

# Ruby

Ruby is a dynamic, open-source programming language with a focus on simplicity and productivity. It has an elegant syntax that is natural to read and easy to write.

## RVM

Install GPG keys:

```Bash
gpg2 --keyserver keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

Install RVM:

```Bash
curl -sSL https://get.rvm.io | bash -s stable
```

Install Ruby:

```Bash
rvm install 3.2.0
```

## Jekyll

Jekyll is a simple, blog-aware static site generator that is ideal for personal, project, or organization sites. It is written in Ruby and requires the RubyGems package manager.

At the root of the project create a `Gemfile` with the following content:

```Bash
source 'https://rubygems.org'
gem 'jekyll'
gem 'bundler'
gem 'json'
gem 'jekyll-theme-cayman'
gem 'html-proofer'
gem 'ffi'
```

Update RubyGems and Bundler, ensure that you have the latest versions of RubyGems and Bundler.
Avoid using sudo when installing gems. If you encounter permission errors, you can use the `--user-install` flag.

```Bash
gem update --system
gem install jekyll bundler
```

Clean the Bundler Environment, remove any existing `Gemfile.lock` file and clean the Bundler environment.

```Bash
rm -f Gemfile.lock
bundle clean --force
```

Configure Bundler to install gems in a specific directory `vendor/bundle` within your project.

```Bash
bundle config set --local path 'vendor/bundle'
```

Install Dependencies, reinstall the dependencies specified in your Gemfile.

```Bash
bundle install
```

Export to PATH in `.zshrc` or `.bashrc`:

```Bash
# Ruby - Jekyll
export PATH=$PATH:$HOME/.rvm/bin
export RUBIES=$HOME/.rvm/rubies
export GEM_HOME=$RUBIES/ruby-3.2.0
export PATH=$PATH:$GEM_HOME/bin
export PATH=$PATH:$HOME/.local/share/gem/ruby/3.0.0/bin
```

Verify Installation, check the installed versions of Jekyll and Bundler.

```Bash
jekyll -v
bundler -v
```

Build the Jekyll site:

```Bash
bundle exec jekyll build
```

Serve the Jekyll site:

```Bash
bundle exec jekyll serve
```

Now, go to `http://localhost:4000` to see the site.

## Keyring

Keyring is a secure way to store credentials for Git. It is a daemon that stores passwords and other sensitive information in encrypted files using the Secret Service API.

### SSH Key Generation

SSH keys are a pair of cryptographic keys that can be used to authenticate to an SSH server as an alternative to password-based logins. One key is private and kept secure on your
machine, while another is public and shared with the server.

```Bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

#### Check the agent is up and running

```Bash
eval "$(ssh-agent -s)"
```

#### Then add it to the ssh-agent

```Bash
ssh-add ~/.ssh/<ssh-key-name>
```

### GPG Key Generation

GPG keys are a pair of cryptographic keys that can be used to sign and encrypt data. One key is private and kept secure on your machine, while another is public and shared with the
server.

Follow the default options (RSA, 4096 bits). Input your personal information: name, email, and passphrase.

```Bash
gpg --full-generate-key
```

#### Add GPG Key

```Bash
gpg --armor --export <GPG Key>
```

#### Check list of GPG keys

```Bash
gpg2 --list-keys --keyid-format LONG
```

```Bash
gpg --list-keys
```

```Bash
gpg --list-secret-keys
```

#### Delete unused keys

```Bash
gpg --delete-keys "User Name"
```

```Bash
gpg --delete-secret-key "User Name"
```

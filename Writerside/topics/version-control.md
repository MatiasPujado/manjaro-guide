# Version Control

GIT:
https://confluence.atlassian.com/bbkb/git-command-returns-fatal-error-about-the-repository-being-owned-by-someone-else-1167744132.html

git config --global --add safe.directory '*' # For the current user and all repositories
sudo git config --system --add safe.directory '*' # For all users and all repositories





The git config command is a convenient way to set configuration options that control all aspects of how Git operates. These configuration options are stored in three different places, each with a different scope:

    System-level (--system): These options are applied to every user on the system and all of their repositories. They are stored in the /etc/gitconfig file. If you pass the --system option to git config, it reads and writes from this file specifically.

    Global-level (--global): These options are applied to all repositories of the current user. They are stored in the .gitconfig or .config/git/config file in the user's home directory. If you pass the --global option to git config, it reads and writes from this file.

    Local-level (--local): These options are applied to the current repository only. They are stored in the .git/config file in the repository's root directory. If you don't specify a level when using git config, this is the default scope.

Each level overrides the values from the previous level, so options in a repository's .git/config file override those in the user's .gitconfig file, which in turn override those in the /etc/gitconfig file

Here's an example of how to set the user name at different levels:

    System-level: sudo git config --system user.name "System User"
    Global-level: git config --global user.name "Global User"
    Local-level: git config --local user.name "Local User"

To see the origin of your configuration, you can use the git config --list --show-origin command.

##sudo git config --system pull.ff true

sudo git config --system core.editor vim
sudo git config --system core.excludesFile /etc/gitignore
sudo git config --system init.defaultBranch main
sudo git config --system pull.rebase true
sudo git config --system credential.helper /usr/lib/git-core/git-credential-libsecret
sudo git config --system user.name "Matias Pujado"
sudo git config --system user.email matiaspujado@gmail.com
sudo git config --system --add safe.directory '*'

git config --global user.name "Matias Pujado"
git config --global user.email matias.pujado@awsoftware.com.ar

git config --global user.name "Matias Pujado"
git config --global user.email matiaspujado@gmail.com

git config --global gpg.program gpg2
git config --global alias.last "log -3 HEAD"
git config --global core.excludesFile /etc/gitignore


# Make SSH Key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Check the agent is up and running
eval "$(ssh-agent -s)"

# Then add it to the ssh-agent
ssh-add ~/.ssh/<ssh-key-name>



# Check list of GPG keys
gpg2 --list-keys --keyid-format LONG

# Make GPG Key
gpg --full-generate-key
- Follow defafault (RCA)
- 4096
- Input your personal info

# Check list of GPG keys again
gpg --list-keys
gpg --list-secret-keys

# Delete unused keys
gpg --delete-keys "User Name"
gpg --delete-secret-key "User Name"

# Add GPG Key
gpg --armor --export <GPG Key>

# Setup ssh Github for JAVA VM

```bash
$ ssh-keygen
```

Set a passphrase and run `cat ~/.ssh/id_rsa.pub`.
Keep your passphrase and ssh key somewhere so you don't lose them.

Go to your project repository and set the origin as SSH connection.
Then we'll setup the ssh configuration.

```bash
$ git remote set-url origin git@github.com:<username>/<project_name>.git
$ sudo nano /etc/ssh/ssh_config
```

! Uncomment if they're commented
- PasswordAuthentification no
- Protocol 2

Finally, reload the ssh service with `sudo service ssh restart`.

# Skip below step if you do not want github cli

Go to your account > Settings > SSH and GPG keys > New SSH key.
Add a meaningful name and paste the id_rsa.pub.
All set.

# Setup Git credential manager and link the id_rsa.pub to GitHub

CLI Release : https://github.com/cli/cli/releases

```bash
cd /tmp
sudo curl -LO https://github.com/cli/cli/releases/latest/download/gh_2.31.0_linux_amd64.deb
sudo dpkg -i gh_2.31.0_linux_amd64.deb
sudo apt-get install -f
sudo rm -rf gh_2.31.0_linux_amd64.deb
```

You've now installer gh, the following command will tell you how to set it up.

```bash
gh --version
gh auth login
```

You'll be prompted with a github login setup, choose the following

- GitHub.com
- SSH
- id_rsa.pub (enter a name for it)
- Paste an authentification token

Go to your GitHub profile > Settings > Developer settings > Personal access tokens > Tokens (classic) > Generate new token
Give the token a meaningful name
Set an expiration of your choice
Select the repo scope + admin:org -> read:org
Generate the token
Save the token as you will never see it again
Paste the token as authentification token

Once logged in.
Return to your root project and run the following to setup git.

```bash
$ git config --global credential.helper gh
```

# Setup SSH Agent for the passphrase

Activate the ssh agent and give it the id_rsa.pub.

```bash
$ eval(ssh-agent -s)
$ ssh-add ~/.ssh/id_rsa
```

Enter your passphrase and done.

You can kill an agent with the following commands :

```bash
$ ssh-agent -k
$ # If it doesn't work
$ pgrep ssh-agent
$ kill <pid>
```

# Setup SSH Agent for convenience

Create a new file called .ssh_pwd_persistance.sh

```bash
$ cd
$ nano .ssh_pwd_persistance.sh
```

And write the following :

```bash
##!/bin/bash

function cd() {
  builtin cd "$@"
  if [ "$(pwd)" = "/home/maghwyn/java-testing" ]; then
    if ! pgrep ssh-agent > /dev/null; then
      eval "$(ssh-agent -s)"
    fi

    if ! ssh-add -l > /dev/null; then
      ssh-add ~/.ssh/id_rsa
    fi
  fi
}
```

Open .bashrc with `nano ~/.bashrc`, and add the following : 

```bash
source ~/.ssh_pwd_persistance.sh
```

Open .bash_logout with `nano ~/.bash_logout`, and add the follwing :

```bash
if [ -n "$SSH_AUTH_SOCK" ] ; then
  eval "/usr/bin/ssh-agent -k"
fi
```
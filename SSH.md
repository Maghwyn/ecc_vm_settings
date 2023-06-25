
# Connect to the VM with SSH and a password

Retrieve the ip of the VM.
Open a terminal on your main machine.
Connect to ssh with `ssh <username>@<ip>.`

# Secure SSH connexion, connect to the VM with SSH and an SSH key

## Create the id_rsa.pub

Open a terminal in your main machine.

```bash
$ ssh-keygen
```

Set a passphrase and run `cat ~/.ssh/id_rsa.pub`.
Keep your passphrase and ssh key somewhere so you don't lose them.

## Setup SSH in your VM

Login as your user, not the root, then go to your user root folder by running the command `cd`.

```bash
$ sudo apt-get -y install ufw
$ sudo ufw enable
$ mkdir .ssh
$ nano authorized_keys
```

Paste the SSH key you generate on your main machine into this new file.
Then, setup the permissions of the ssh folder.

```bash
$ cd ..
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```

Finally, allow the ssh throught the firewall.

```bash
$ sudo ufw allow from <main machine ip> to any port 22
```

Then we'll also update the SSHd configuration

```bash
$ sudo nano /etc/ssh/sshd_config
```

! Uncomment if they're commented
- PermitRootLogin no
- PasswordAuthentification no
- Protocol 2 (add it if it doesn't exist in sshd_config)

Finally, reload the ssh service with `sudo service ssh restart`.

## Fix firewall for PhpMyAdmin VM

```bash
$ sudo ufw allow http
$ sudo ufw allow https
```

## Fix firewall for JAVA VM

```bash
$ sudo ufw allow http
$ sudo ufw allow https
$ sudo ufw allow <port>
```

# Setup ssh Github for JAVA VM

Go to your github account > Settings > SSH and GPG keys > New SSH key
Now go to your JAVA VM

```bash
$ ssh-keygen
```

Set a passphrase and run `cat ~/.ssh/id_rsa.pub`.
Keep your passphrase and ssh key somewhere so you don't lose them.

Now on your github account, give a name to your ssh and paste the id_rsa.pub, then save.

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
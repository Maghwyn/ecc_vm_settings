
## Connect to the VM with SSH and a password

Retrieve the ip of the VM.
Open a terminal on your main machine.
Connect to ssh with `ssh <username>@<ip>.`

## Secure SSH connexion, connect to the VM with SSH and an SSH key

### Create the id_rsa.pub

Open a terminal in your main machine.

```bash
$ ssh-keygen
```

Set a passphrase and run `cat ~/.ssh/id_rsa.pub`.
Keep your passphrase and ssh key somewhere so you don't lose them.

### Setup SSH in your VM

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

Then we'll also update the SSH configuration

```bash
$ sudo nano /etc/ssh/sshd_config
$ sudo nano /etc/ssh/ssh_config
```

For each files.

! Uncomment if they're commented
- PermitRootLogin no (add it if it doesn't exist in ssh_config)
- PasswordAuthentification no
- Protocol 2 (add it if it doesn't exist in sshd_config)

Finally, reload the ssh service with `sudo service ssh restart`.
All set.
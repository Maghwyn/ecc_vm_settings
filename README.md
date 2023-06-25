---
- VirtualBox
- Debian 10 x64
- Bridged Network
- SSH Server Only
- Pc OS : Linux
---

# General setup

## Users account

- Root user name : root
- Root user pwd : root

- Admin user name : admin
- Admin user pwd : admin

Note : You can change the passwords of users with `passwd <user>`, a prompt will ask for a password confirm then a new password.

## Sudo Privilege

Login as root user.

```bash
$ apt update
$ apt-get -y install sudo
$ usermod -aG <user>
```

Then logout as root and login as admin.

## Retrieve your VM ip

```bash
$ ip a | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'
```

## Required Packages

```bash
$ sudo apt update
$ sudo apt-get -y install wget
$ sudo apt-get -y install curl
```
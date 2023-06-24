# Install MySQL software Repository

```bash
$ sudo apt-get -y install gnupg
$ cd /tmp
$ sudo wget https://dev.mysql.com/get/mysql-apt-config_0.8.25-1_all.deb
$ sudo apt install ./mysql-apt-config_0.8.25-1_all.deb
$ sudo apt update
$ rm -rf mysql-apt-config_0.8.25-1_all.deb
```

# Install MySql

Install mysql-server with the command `sudo apt-get -y install mysql-server`, you'll be prompted to enter a password for the root user. Then you can check if the service is running with `sudo systemctl status mysql`.

# Secure MySql

```bash
$ mysql_secure_installation
```

First, enter the password of your root user from mysqsl.
Second, you'll need to choose a security level for password, use the strongest.
Third, a prompt will ask you if you want to change your root password. If your root password is of low security, change it.
Lastly, for the rest you can press yes. It will remove the "dev/testing" setup of mysql which we don't need.

If you want to test the successful instalation of mysql, run `mysqladmin -u root -p version`.

# Create a Mysql User

```bash
$ sudo mysql
$ sudo mysql -u root -p
```

Enter your password, then create the user of your choice with the following queries :
Note : It's better to use the `ip` as PhpMyAdmin is on another VM, run `ip a` to retrieve it.

```sql
> CREATE USER '<username>'@'<localhost or ip>' IDENTIFIED WITH caching_sha2_password BY '<password>';
> GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO '<username>'@'<localhost/ip>' WITH GRANT OPTION;
> FLUSH PRIVILEGES;
> exit
```

Note: If you used localhost for the host and want to change it later, run the following : `UPDATE mysql.user SET Host='<new host>' WHERE User='<username>' AND Host='<old host>';`

Now you can connect to your user with `sudo mysql -u <username> -p`.

# Link SQL VM with PhpMyAdmin VM

WARNING : The user host must be the ip of the SQL VM.
Use `ip a` to retrieve it.

```bash
$ sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Add `bind-address = <mysql vm ip>` after `log-error`, then restart mysql.

```bash
$ sudo service mysql restart
$ sudo systemctl restart mysql.service
```
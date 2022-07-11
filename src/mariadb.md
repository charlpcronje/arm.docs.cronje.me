---: Install MariaDB
---
# Install MariaDB on Raspberry Pi

Maria DB is used by so many other software so better just to get it installed now

Run the following commands

```sh
sudo su
apt-get install mariadb-server
apt-get install mariadb-client
```

Now you can check if it is working

```sh
mysql

# mysql>

exit
```

Now secure your install

```sh
sudo mysql_secure_installation
```

follow the instructions

Done
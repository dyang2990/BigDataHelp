# Reset Ambari Admin Password

```
sudo ambari-admin-password-reset
```

# Reset MySQL root password

```
sudo systemctl stop mysqld
sudo systemctl set-environment MYSQLD_OPTS="--skip-grant-tables --skip-networking"
sudo systemctl start mysqld
mysql -uroot
```

> Once in Mysql is up and running

```
FLUSH PRIVILEGES;
SET PASSWORD FOR 'root'@'localhost'=PASSWORD('hadoop');
FLUSH PRIVILEGES;
quit;
sudo systemctl unset-environment MYSQLD_OPTS
sudo systemctl restart mysqld
mysql -uroot -phadoop
```

### unzip and untar

```
sudo unzip file.zip
sudo tar xvf file.tar.gz
```


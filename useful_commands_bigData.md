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

### delete files in AWS S3 Bitbucket
```
import java.net.URI
import org.apache.hadoop.fs.FileSystem
import org.apache.hadoop.fs.Path

FileSystem.get(new URI("s3n://bucket"), sc.hadoopConfiguration).delete(new Path("s3n://bucket/path_to_delete"), true)
```

## mysql 数据库备份还原

直接在命令行执行 `mysqldump -uroot -proot --database elk --compact > elk.sql` 导出文件到 elk.sql  
添加压缩： `mysqldump -uroot -proot --database elk --compact | gzip > /tmp/test.gzip`  
还原： `mysql -u root -p ` 进入控制台, `usr elk`切换库， `source d:\elk.sql`进行还原。`gunzip -fc /tmp/test.gz | mysql -u[USER] -h[HOST] -P[PORT] DB`解压缩并还原。

## mysql 日志时间差 8 小时， 将 UTC 改为本地时间

SHOW VARIABLES LIKE 'log_timestamps';
SET GLOBAL log_timestamps = SYSTEM;

### my.cnf配置  
路径 
/etc/my.cnf    默认  
/etc/my.cnf.d/    
/etc/mysql/conf. 
/etc/mysql/mysql.conf.d
```cnf
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
skip-host-cache
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=/var/lib/mysql-files
user=mysql

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

#log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
# 
log_timestamps=system
[client]
socket=/var/run/mysqld/mysqld.sock

!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/

```

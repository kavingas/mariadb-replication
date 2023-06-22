Docker MySQL master-slave replication 
========================


#### run on master to create replication user

```bash
CREATE USER IF NOT EXISTS "mydb_slave_user"@"%" IDENTIFIED BY "mydb_slave_pwd"; GRANT REPLICATION SLAVE ON *.* TO "mydb_slave_user"@"%"; FLUSH PRIVILEGES;
```
#### find the position and file
```bash
MariaDB [(none)]> SHOW MASTER STATUS;
+----------+----------+--------------+------------------+
| File     | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+----------+----------+--------------+------------------+
| 1.000003 | 46918641 |              |                  |
+----------+----------+--------------+------------------+
1 row in set (0.01 sec)
```

#### run on slave to start replication
```bash
CHANGE MASTER TO MASTER_HOST ='mysql_master',MASTER_USER ='mydb_slave_user',MASTER_PASSWORD ='mydb_slave_pwd',MASTER_LOG_FILE ='<File>',MASTER_LOG_POS =<Position>;
START SLAVE;
```
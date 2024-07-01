\## <p align="center">How to Install MySQL 9.0 on Rocky Linux 9.4</p>



```sql
[root@mydb ~]# cat /etc/redhat-release
Rocky Linux release 9.4 (Blue Onyx)

[root@mydb ~]# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 9.0.0 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select version();
+-----------+
| version() |
+-----------+
| 9.0.0     |
+-----------+
1 row in set (0.00 sec)

mysql>
```


\## <p align="center">How to migrate and upgrade from MySQL 5.7 to MySQL 8.0</p>



Recently, there are many projects that upgrade and migrate mysql 5.7 to 8.0, and there are many ways to achieve this goal. 



#### 1、Will mysqldump have a problem?

```
mysql> grant all privileges on *.* to test_user@'%';
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
mysql> select * from performance_schema.replication_applier_status_by_worker\G
*************************** 1. row ***************************
                                           CHANNEL_NAME:
                                              WORKER_ID: 1
                                              THREAD_ID: NULL
                                          SERVICE_STATE: OFF
                                      LAST_ERROR_NUMBER: 1726
                                     LAST_ERROR_MESSAGE: Worker 1 failed executing transaction 'ANONYMOUS' at source log mydb-binlog.000019, end_log_pos 520; Error 'Storage engine 'MyISAM' does not support system tables. [mysql.user]' on query. Default database: ''. Query: 'GRANT ALL PRIVILEGES ON *.* TO 'test_user'@'%''
```



#### 2、Clever use of copyInstance

```
 MySQL  localhost  JS > util.copyInstance('mysql://root@192.168.5.140:3306',{threads: 6, deferTableIndexes: "all", excludeUsers:['root@localhost']})
Copying DDL, Data and Users from in-memory FS, source: mydb01:3306, target: mydb02:3306.
NOTE: SRC: Backup lock is not supported in MySQL 5.7 and DDL changes will not be blocked. The dump may fail with an error if schema changes are made while dumping.
SRC: Acquiring global read lock
SRC: Global read lock acquired
Initializing - done
SRC: 2 out of 6 schemas will be dumped and within them 37 tables, 7 views, 6 routines, 6 triggers.
SRC: 3 out of 6 users will be dumped.
Gathering information - done
SRC: All transactions have been started
SRC: Global read lock has been released
SRC: Writing global DDL files
SRC: Writing users DDL
SRC: Running data dump using 6 threads.
NOTE: SRC: Progress information uses estimated values and may not be accurate.
TGT: Opening dump...
NOTE: TGT: Dump is still ongoing, data will be loaded as it becomes available.
TGT: Target is MySQL 8.0.39. Dump was produced from MySQL 5.7.42-log
NOTE: TGT: Destination MySQL version is newer than the one where the dump was created.
TGT: Scanning metadata...
TGT: Scanning metadata - done
TGT: Checking for pre-existing objects...
TGT: Executing common preamble SQL
TGT: Executing DDL...
Writing schema metadata - done
Writing DDL - done
Writing table metadata - done
SRC: Starting data dump
TGT: Executing DDL - done
TGT: Executing user accounts SQL...
TGT: Executing view DDL...
TGT: Executing view DDL - done
TGT: Loading data...
TGT: Starting data load
100% (247.27K rows / ~245.23K rows), 40.22K rows/s, 8.04 MB/s
SRC: Dump duration: 00:00:06s
SRC: Total duration: 00:00:06s
SRC: Schemas dumped: 2
SRC: Tables dumped: 37
SRC: Data size: 41.01 MB
SRC: Rows written: 247271
SRC: Bytes written: 41.01 MB
SRC: Average throughput: 6.72 MB/s
2 thds loading - 1 thds indexing | 100% (41.01 MB / 41.01 MB), 7.97 MB/s (44.08K rows/s), 35 / 37 tables done
Recreating indexes - done
TGT: Executing common postamble SQL
TGT: 37 chunks (247.27K rows, 41.01 MB) for 37 tables in 2 schemas were loaded in 5 sec (avg throughput 7.80 MB/s, 47.00K rows/s)
TGT: 53 DDL files were executed in 0 sec.
TGT: 3 accounts were loaded
TGT: Data load duration: 5 sec
TGT: 46 indexes were recreated in 2 sec.
TGT: Total duration: 8 sec
TGT: 0 warnings were reported during the load.

---
Dump_metadata:
  Binlog_file: mydb-binlog.000023
  Binlog_position: 154
  Executed_GTID_set: ''

```



```
mysql> CHANGE REPLICATION SOURCE TO SOURCE_HOST="192.168.5.130",SOURCE_USER="repuser",SOURCE_PASSWORD="repuser123",SOURCE_LOG_FILE='mydb-binlog.000023',SOURCE_LOG_POS=154;
Query OK, 0 rows affected, 2 warnings (0.03 sec)

mysql> start replica;
Query OK, 0 rows affected (0.21 sec)
```



```
mysql> pager grep Running
PAGER set to 'grep Running'
mysql> show replica status\G
           Replica_IO_Running: Yes
          Replica_SQL_Running: Yes
    Replica_SQL_Running_State: Replica has read all relay log; waiting for more updates
1 row in set (0.00 sec)

mysql> nopager
PAGER set to stdout
```


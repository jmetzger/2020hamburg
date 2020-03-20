# 2020 MariaDB Galera Cluster Training Hamburg (Remote)

## Read-Write-Split 

https://www.percona.com/blog/2016/03/29/read-write-split-routing-performance-in-maxscale/

## Monitoring 

https://galeracluster.com/library/documentation/monitoring-cluster.html

## maxscale 

When a server that readwritesplit uses is put into maintenance mode, any ongoing requests are allowed to finish before the connection is closed

## Use performance schema for slow queries 

performance_schema needs to be on:

``
MariaDB [performance_schema]> show variables like '%perf%schema';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| performance_schema | ON    |
+--------------------+-------+
1 row in set (0.001 sec)

MariaDB [performance_schema]> 

``

This needs to be set:

``
MariaDB [performance_schema]> select * from setup_consumers where name like 'stat%' or name like '%instru%';
+------------------------+---------+
| NAME                   | ENABLED |
+------------------------+---------+
| global_instrumentation | YES     |
| thread_instrumentation | YES     |
| statements_digest      | YES     |
+------------------------+---------+
3 rows in set (0.001 sec)
``

Get all queries by time 

``
MariaDB [performance_schema]> SELECT SCHEMA_NAME, digest, digest_text, round(sum_timer_wait/ 1000000000000, 6), count_star FROM performance_schema.events_statements_summary_by_digest ORDER BY sum_timer_wait DESC LIMIT 10;
``

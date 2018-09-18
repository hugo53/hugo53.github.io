---
layout: post
title: "Several notes on SQL Limitation"
description: "Notes on MySQL, MongoDB Limitation"
category: 
tags: [database, peformance]
---
{% include JB/setup %}

## MySQL
- Row Limit: [65535 bytes](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.7/en/column-count-limit.html)
```
mysql> CREATE TABLE t (a VARCHAR(10000), b VARCHAR(10000),
       c VARCHAR(10000), d VARCHAR(10000), e VARCHAR(10000),
       f VARCHAR(10000), g VARCHAR(6000)) ENGINE=InnoDB CHARACTER SET latin1;
ERROR 1118 (42000): Row size too large. The maximum row size for the used 
table type, not counting BLOBs, is 65535. This includes storage overhead, 
check the manual. You have to change some columns to TEXT or BLOBs
```
```
mysql> CREATE TABLE t (a VARCHAR(10000), b VARCHAR(10000),
       c VARCHAR(10000), d VARCHAR(10000), e VARCHAR(10000),
       f VARCHAR(10000), g VARCHAR(6000)) ENGINE=MyISAM CHARACTER SET latin1;
ERROR 1118 (42000): Row size too large. The maximum row size for the used 
table type, not counting BLOBs, is 65535. This includes storage overhead, 
check the manual. You have to change some columns to TEXT or BLOBs
```

#### Solution
Use TEXT or BLOB instead of.
```
mysql> CREATE TABLE t (a VARCHAR(10000), b VARCHAR(10000),
       c VARCHAR(10000), d VARCHAR(10000), e VARCHAR(10000),
       f VARCHAR(10000), g TEXT(6000)) ENGINE=MyISAM CHARACTER SET latin1;
Query OK, 0 rows affected (0.02 sec)
```

- Table Limit ([InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-restrictions.html#innodb-maximums-minimums))

    InnoDB Page Size ->	Maximum Tablespace Size

    4KB -> 16TB

    8KB -> 32TB

    16KB ->	64TB (Default InnoDB page size is 16KB)

    32KB ->	128TB

    64KB ->	256TB 


At default setting, maximum table size is 64TB, with 65535 bytes row limit, then should store 1,073,741,824 rows in the table.

## MongoDB
- Document size limit: [16MB](https://docs.mongodb.com/manual/reference/limits/#bson-documents)
- No specific limition on table size with WiredTiger storage engine, only depend on hardware disk capacity and sometimes RAM capacity
- MMAPv1 storage engine has lots of limitations but [it's depreciated from MongoDB 4.0 and will be removed in a future release](https://docs.mongodb.com/manual/core/mmapv1/).

## References
- [https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.7/en/column-count-limit.html](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.7/en/column-count-limit.html)
- [https://dev.mysql.com/doc/refman/5.7/en/innodb-restrictions.html#innodb-maximums-minimums](https://dev.mysql.com/doc/refman/5.7/en/innodb-restrictions.html#innodb-maximums-minimums)
- [https://docs.mongodb.com/manual/reference/limits/](https://docs.mongodb.com/manual/reference/limits/)
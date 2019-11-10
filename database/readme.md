# Database

SELECT COUNT(*)를 빠르게 하는 방법

|<center>Index</center>|<center>Query</center>|<center>Comment</center>|
|:--------------------:|:--------------------:|:----------------------:|
|1|SELECT COUNT(*) FROM Transactions|Performs a full table scan. Slow on large tables.|
|2|SELECT CONVERT(bigint, rows) FROM sysindexes WHERE id = OBJECT_ID('Transactions') AND indid < 2|Fast way to retrieve row count. Depends on statistics and is inaccurate. Run DBCC UPDATEUSAGE(Database) WITH COUNT_ROWS, which can take significant time for large tables.|
|3|SELECT CAST(p.rows AS float) FROM sys.tables AS tbl INNER JOIN sys.indexes AS idx ON idx.object_id = tbl.object_id and idx.index_id < 2 INNER JOIN sys.partitions AS p ON p.object_id=CAST(tbl.object_id AS int) AND p.index_id=idx.index_id WHERE ((tbl.name=N'Transactions' AND SCHEMA_NAME(tbl.schema_id)='dbo'))|The way the SQL management studio counts rows (look at table properties, storage, row count). Very fast, but still an approximate number of rows.|
|4|SELECT SUM (row_count) FROM sys.dm_db_partition_stats WHERE object_id=OBJECT_ID('Transactions') AND (index_id=0 or index_id=1);|Quick (although not as fast as method 2) operation and equally important, reliable.|

참조 링크
- [select count(*) 를 빠르게 하는 방법](https://paulus78.tistory.com/entry/select-count-를-빠르게-하는-방법)

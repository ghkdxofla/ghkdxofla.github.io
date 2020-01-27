# SELECT COUNT(*)를 빠르게 하는 방법

|<center>Index</center>|<center>Query</center>|<center>Comment</center>|
|:--------------------:|:--------------------:|:----------------------:|
|1|SELECT COUNT(*) FROM Transactions|Performs a full table scan. Slow on large tables.|
|2|SELECT CONVERT(bigint, rows) FROM sysindexes WHERE id = OBJECT_ID('Transactions') AND indid < 2|Fast way to retrieve row count. Depends on statistics and is inaccurate. Run DBCC UPDATEUSAGE(Database) WITH COUNT_ROWS, which can take significant time for large tables.|
|3|SELECT CAST(p.rows AS float) FROM sys.tables AS tbl INNER JOIN sys.indexes AS idx ON idx.object_id = tbl.object_id and idx.index_id < 2 INNER JOIN sys.partitions AS p ON p.object_id=CAST(tbl.object_id AS int) AND p.index_id=idx.index_id WHERE ((tbl.name=N'Transactions' AND SCHEMA_NAME(tbl.schema_id)='dbo'))|The way the SQL management studio counts rows (look at table properties, storage, row count). Very fast, but still an approximate number of rows.|
|4|SELECT SUM (row_count) FROM sys.dm_db_partition_stats WHERE object_id=OBJECT_ID('Transactions') AND (index_id=0 or index_id=1);|Quick (although not as fast as method 2) operation and equally important, reliable.|

# JOIN의 종류와 사용법
두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법
INNER JOIN
- 교집합
- 기준 테이블과 JOIN한 테이블의 중복된 값
- 결과값은 A의 테이블과 B 테이블이 모두 가지고 있는 데이터만 검색
- (  A  ( R )  B  )
```sql
--문법--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
INNER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키....

--예제--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A
INNER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP AND A.DEPT = B.DEPT
```
LEFT OUTER JOIN
- 기준 테이블의 값 + 테이블과 기준 테이블의 중복된 값
- (  A R  ( R )  B  )
```sql
--문법--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
LEFT OUTER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키 .....

--예제--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A
LEFT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP AND A.DEPT = B.DEPT
```
RIGHT OUTER JOIN
- (  A  ( R )  B R  )
```sql
--문법--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
RIGHT OUTER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키 .....

--예제--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A
RIGHT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP AND A.DEPT = B.DEPT
```
FULL OUTER JOIN
- (  A R  ( R )  B R  )
```sql
--문법--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
FULL OUTER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키 .....

--예제--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A
FULL OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP AND A.DEPT = B.DEPT
```
CROSS JOIN
- A의 데이터 * B의 데이터
```sql
--문법(첫번째방식)--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
CROSS JOIN 조인테이블 별칭

--예제(첫번째방식)--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A
CROSS JOIN JOIN_TABLE B

=====================================================================================

--문법(두번째방식)--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭,조인테이블 별칭

--예제(두번째방식)--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A,JOIN_TABLE B
```
SELF JOIN
```sql
--문법--
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 테이블 별칭,테이블 별칭2

--예제--
SELECT
A.NAME, --A테이블의 NAME조회
B.AGE --B테이블의 AGE조회
FROM EX_TABLE A,EX_TABLE B
```

# Row Number를 매기는 방법(MSSQL)
```sql
SELECT * FROM ( SELECT ROW_NUMBER() OVER(ORDER BY idx) rownum, * FROM page_table ) page_table WHERE rownum BETWEEN 1 AND 20
```

# pyodbc에서 Stored Procedure가 제대로 실행 또는 반영이 안될 경우
```sql
with engine.connect() as con:
    # 아래의 설정을 추가
    con.execution_options(autocommit=True, isolation_level='READ UNCOMMITTED')
    # SET NOCOUNT ON을 선으로 실행
    # 추가적으로 @muted = 1을 넣는지 여부는 분석 중
    con.execute('SET NOCOUNT ON; {your procedure code here} @param_1 = 'variable')
```

# 참조 링크
[select count(*) 를 빠르게 하는 방법](https://paulus78.tistory.com/entry/select-count-를-빠르게-하는-방법)
[페이징 쿼리](https://roqkffhwk.tistory.com/146)
[sqlalchemy - Stored Procedure](https://docs.sqlalchemy.org/en/13/core/connections.html)
[sqlalchemy - SET NOCOUNT ON](https://stackoverflow.com/questions/24458430/make-python-wait-for-stored-procedure-to-finish-executing)
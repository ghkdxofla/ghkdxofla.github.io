# Row Number를 매기는 방법
```sql
SELECT * FROM ( SELECT ROW_NUMBER() OVER(ORDER BY idx) rownum, * FROM page_table ) page_table WHERE rownum BETWEEN 1 AND 20
```

# 문자열의 시작 위치 찾기
```sql
SELECT CHARINDEX('찾을문자열A','지정문자열B')
SELECT CHARINDEX('찾을문자열A','지정문자열B',숫자C) -- 이건 C 위치에서부터 B에서 A를 찾으라는 뜻
```

# MSSQL Stored procedure의 각종 함수 예시

선언과 초기화
```sql
-- 선언
DECLARE @HTL DATETIME

-- 초기화
SET @HTL=GETDATE()
```

조건문 관련
```sql
-- 조건문
IF 조건
    실행문
ELSE
    실행문

-- 실행문이 2개 이상인 조건문
IF 조건
    BEGIN
        실행문 1
        실행문 2
        ...
    END

-- 조건문의 조건
SELECT * FROM [테이블명] WHERE [칼럼명] = [조건] OR 칼럼명 = [조건1] OR 칼럼명 = [조건2] OR 칼럼명 = [조건3] --OR
-- IN을 통해 여러 OR 조건이 있는 sql 문장을 OR와 동일하게 사용 가능하다
SELECT * FROM [테이블명] WHERE [칼럼명] IN =( [조건1],[조건2],[조건3] ) --IN

-- case문(JOIN한 것과 같은 결과)
SELECT A, B, C, (
    CASE
        WHEN B=10
            THEN 'b'
        WHEN B=20
            THEN 'c'
        ELSE
            'd'
    END) AS K
    FROM ALPHA

-- 중첩 case문 예시
SELECT  T1.STORE_ID
        ,T1.STORE_ADDR
        ,CASE WHEN T1.STORE_SIZE >= 100 THEN
                CASE WHEN T2.REGION_GD IN ('S') THEN 'High grade'
                     WHEN T2.REGION_GD IN ('A','B') THEN 'Mid Grade'
                     ELSE 'Low Grade'
                END
              WHEN T1.STORE_SIZE >= 50 THEN
                CASE WHEN T2.REGION_GD IN ('S', 'A') THEN 'High Grade'
                     WHEN T2.REGION_GD IN ('B') THEN 'Mid Grade'
                     ELSE 'Low Grade'
                END
             ELSE
                CASE WHEN T2.REGION_GD IN ('S', 'A', 'B') THEN 'High Grade'
                     ELSE 'Low Grade'
                END
        END STORE_SIZE_GD
FROM    SQL_TEST.MA_STORE T1
        ,SQL_TEST.CD_REGION T2
WHERE   T1.REGION_CD = T2.REGION_CD

-- DECODE
-- 조건에 따라 데이터를 다른 값이나 컬럼값으로 추출 할 수 있다
-- DECODE(VALUE, IF1, THEN1, IF2, THEN2...) 형태로 사용 할 수 있다
-- VALUE 값이 IF1일 경우에 THEN1 값을 반환하고, VALUE 값이 IF2일 경우에는 THEN2 값을 반환한다
-- DECODE 함수 안에 DECODE함수를 중첩으로 사용 할 수 있다
SELECT deptno, DECODE(deptno, 10 , 'ACCOUNTING' ,
                              20 , 'RESEARCH' ,
                              30 , 'SALES', 'OPERATIONS') name
FROM dept;

-- 결과
DEPTNO   NAME
------    ----------
    10    ACCOUNTING
    20    RESEARCH
    30    SALES
    40    OPERATIONS
```

반복문
```sql
-- 반복문
WHILE @A < 100
    BEGIN
        실행문
    END
```

문자열 치환
```sql
-- 문자열 치환
-- REPLACE
-- REPLACE(대상, 바꿀 문자, 바뀔 문자);
SELECT REPLACE('abc123abc', 'abc', '321');
--> 321123321

-- STUFF
-- STUFF(대상, 치환 시작 위치, 치환 문자 수, 바뀔 문자);
SELECT STUFF('abcdef', 2, 3, 'XXX');
--> aXXXef
```

# 참조 링크
[문자열의 시작 위치 찾기](https://kmj1107.tistory.com/entry/MSSQL-CharIndex-%EB%AC%B8%EC%9E%90%EC%97%B4%EC%9D%98-%EC%8B%9C%EC%9E%91-%EC%9C%84%EC%B9%98-%EC%B0%BE%EA%B8%B0)
[Procedure 간단한 명령어들](https://hklovecw.tistory.com/7)
[CASE...WHEN...THEN, DECODE](https://devbox.tistory.com/entry/DBMS-CASEWHENTHEN)
# Row Number를 매기는 방법
```sql
SELECT * FROM ( SELECT ROW_NUMBER() OVER(ORDER BY idx) rownum, * FROM page_table ) page_table WHERE rownum BETWEEN 1 AND 20
```

# 문자열의 시작 위치 찾기
```sql
SELECT CHARINDEX('찾을문자열A','지정문자열B')
SELECT CHARINDEX('찾을문자열A','지정문자열B',숫자C) -- 이건 C 위치에서부터 B에서 A를 찾으라는 뜻
```

# MSSQL Stored procedure 간단 명령어들
```sql
-- 선언
DECLARE @HTL DATETIME

-- 초기화
SET @HTL=GETDATE()

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

-- 반복문
WHILE @A < 100
    BEGIN
        실행문
    END
```








# 참조 링크
[문자열의 시작 위치 찾기](https://kmj1107.tistory.com/entry/MSSQL-CharIndex-%EB%AC%B8%EC%9E%90%EC%97%B4%EC%9D%98-%EC%8B%9C%EC%9E%91-%EC%9C%84%EC%B9%98-%EC%B0%BE%EA%B8%B0)
[Procedure 간단한 명령어들](https://hklovecw.tistory.com/7)
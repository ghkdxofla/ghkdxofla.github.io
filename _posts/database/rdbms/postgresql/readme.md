# 설치(Ubuntu)
```bash
# postgresql 설치
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib

# postgresql 계정 변경
sudo -i -u postgres

# postgresql 접속
psql

# sudo 계정으로 접속
sudo -u postgres psql

# 새로운 계정 생성
sudo -u postgres
createuser --interactive

# 데이터베이스 생성
createdb {db_name, ex: dejavu}

# postgresql은 데이터베이스 명과 동일한 linux 유저 계정이 필요하다
sudo adduser {user_name, ex: dejavu}

# 생성한 데이터베이스에 접속
sudo -u {user_name, ex: dejavu} psql -d {db_name, ex: dejavu}

# unixodbc 설치(pyodbc 등 사용 시)
sudo apt-get install unixodbc unixodbc-dev odbc-postgresql

# 파일 확인
ls /usr/lib64/psqlodbcw* -l

# odbc.ini 위치 확인
odbcinst -j

# odbc.ini 수정
[My PostgreSQL ANSI Connection name]
Driver     = /usr/local/lib/psqlodbca.so
Servername = your_server_name <network name or IP address for the server>
Port       = 5432 <5432 = default - a different port can be used>
Database   = dbname <if omitted the default database will be used>
Username   = dbusername <if omitted then integrated security is used - needs kerberos>
Password   = dbpassword

```


# 참조 링크
[Ubuntu에 PostgreSQL 설치하기](https://dejavuqa.tistory.com/16)
[Connect to PostgreSQL from Linux with ODBC](https://help.interfaceware.com/v6/connect-to-postgresql-from-linux-or-mac-with-odbc)
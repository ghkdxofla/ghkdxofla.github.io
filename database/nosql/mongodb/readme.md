# Mongo DB란?
간단한 정의
- NoSQL 데이터베이스로, 기존 RDBMS의 SQL을 사용하는 방식이 아님
- 테이블 간 관계가 RDBMS와 다르며, ACID 방식으로 작동하지 않음

# Local 환경의 mount된 disk를 활용한 sharding 구축
```bash
# 구성은 2개의 샤드 서버, 2개의 config 서버(1개는 replica)
# 

# Shard server
mongod --shardsvr --dbpath {path} --port 30001
mongod --shardsvr --dbpath {path} --port 30002

# Config server
mongod --configsvr --replSet {replica name} --dbpath {path/config} --port 50001
mongod --configsvr --replSet {replica name} --dbpath {path/config} --port 50002

# Init mongos
mongos --configdb {replica name}/localhost:50001,localhost:50002 --port 27017

# Add sharding server
# mongos 접속 후 입력
mongos> sh.addShard("localhost:30001");
mongos> sh.addShard("localhost:30002");
mongos> sh.addShard("localhost:30003");

# Check sharding server's status
mongos> sh.status()

# Enable database and collection at sharding server
mongos> sh.enableSharding("{database}")
mongos> sh.shardCollection("{database}.{collection}", {_id: "hashed"})
```

# MongoDB Service가 system boot 시 자동으로 시작하도록 변경
```bash
systemctl enable mongod.service
# or
systemctl enable mongodb.service

# 위의 두 개가 안될 경우
sudo vi /etc/systemd/system/mongodb.service

# Edit service file
[Unit]
Description=MongoDB Database Service
Wants=network.target
After=network.target

[Service]
ExecStart=/usr/bin/mongod --config /etc/mongod.conf
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
User=mongodb
Group=mongodb
StandardOutput=syslog
StandardError=syslog

# Restart mongod
sudo service mongod start|stop|restart
```

# 참조 링크
[How to install mongodb on debian 9 - 1](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-debian-9)
[How to install mongodb on debian 9 - 2](https://linuxize.com/post/how-to-install-mongodb-on-debian-9/)
[MongoDB 샤딩 적용하기](https://sudarlife.tistory.com/entry/mongodb-Sharding-%EB%AA%BD%EA%B3%A0%EB%94%94%EB%B9%84-%EC%83%A4%EB%94%A9-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0-config-sever-replica-set)
[Sharding methods](https://docs.mongodb.com/manual/reference/method/js-sharding/)
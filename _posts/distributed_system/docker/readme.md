### Docker란?

### 설치
```bash
### Update repository
$ sudo apt-get update

### Install requirements
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

### Download apt-key
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

### Verificate apt-key
$ sudo apt-key fingerprint 0EBFCD88

### Add repository to local environment
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

### Install docker
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### docker-compose란?

### docker-compose.yml 작성 예시

### docker-comppose에 여러 개의 image를 설치하는 법
```yaml
### build is your actual build spec
build:
  image: myrepo/myimage
  build:
  ...
  ...
### these extend from build and just add new tags statically or from environment variables or 
version_tag:
  extends: build
  image: myrepo/myimage:v1.0
some_other_tag:
  extends: build
  image: myrepo/myimage:${SOME_OTHER_TAG}
```

### dockerfile 예시
```dockerfile
### Install base image(node.js)
FROM node:12
MAINTAINER taelim.hwang@lge.com

### Set work directory
WORKDIR /usr/src/app

### Update repository
RUN apt-get -y update

### Upgrade repository
RUN apt-get -y dist-upgrade

### Install sudo
RUN apt-get -y install sudo

### Install nvm
RUN apt-get -y install build-essential libssl-dev
RUN curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh
RUN bash install_nvm.sh
#RUN . ~/.profile
#RUN nvm install 10.15.3

### Install mongodb
RUN wget -qO - https://www.mongodb.org/static/pgp/server-4.0.asc | apt-key add -
RUN echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.0 main" | tee /etc/apt/sources.list.d/mongodb-org-4.0.list
RUN apt-get update
RUN apt-get -y install mongodb-org

### Install mariadb
RUN apt-get -y install mariadb-server

### Install imagemagick
RUN apt-get -y install imagemagick

### Install ffmpeg
RUN apt-get -y install ffmpeg

### Install bower
RUN npm install -g bower

### Install gulp
RUN npm install -g gulp

### Install ruby
RUN apt-get -y install ruby-full build-essential
RUN gem install sass

### Install pm2
RUN npm install -g pm2
RUN apt-get -y dist-upgrade

### Install python2.7
RUN npm install --python=python2.7

### Copy sources
ADD maple-cms-api ./maple-cms-api
ADD maple-cms-api-gateway ./maple-cms-api-gateway
ADD maple-cms-apps ./maple-cms-apps
ADD maple-cms-content ./maple-cms-content
ADD maple-cms-db ./maple-cms-db
ADD maple-cms-storage ./maple-cms-storage
ADD maple-cms-player ./maple-cms-player
ADD maple-cms-display ./maple-cms-display
ADD maple-cms-frontend ./maple-cms-frontend

### Install node packages
RUN npm install --prefix ./maple-cms-api
RUN npm install --prefix ./maple-cms-api-gateway
RUN npm install --prefix ./maple-cms-content
RUN npm install --prefix ./maple-cms-db
RUN npm install --prefix ./maple-cms-player
RUN npm install --prefix ./maple-cms-display
RUN npm install --prefix ./maple-cms-frontend

### Set environment
ENV PYTHONPATH "${PYTHONPATH}:/usr/local/bin"
ENV PATH "${PATH}:/usr/local/bin"

### Set port
EXPOSE 80
EXPOSE 443
EXPOSE 1337
EXPOSE 1443
EXPOSE 8055
EXPOSE 9010
EXPOSE 9020
EXPOSE 9030

### Run API
CMD cp ./maple-cms-db/config/mongod.conf /etc/ && \
mongod --fork --config /etc/mongod.conf && \
service mysql start && \
#sed -i "s|bind_ip|#bind_ip|g" /etc/mongodb.conf && \
mongo --eval "db=db.getSiblingDB('admin');db.createUser({'user':'root','pwd':'lge123','roles':['userAdminAnyDatabase','dbAdminAnyDatabase','readWriteAnyDatabase']})" && \
mongod -f /etc/mongod.conf --shutdown && \
mongod --fork --config /etc/mongod.conf && \
mongo -u 'root' -p 'lge123' --authenticationDatabase 'admin' --eval "db=db.getSiblingDB('mapleLog');db.createUser({'user':'maple_user','pwd':'maple123','roles':['readWrite','dbAdmin']})" && \
mysql -u"root" -e"SET GLOBAL innodb_large_prefix = ON;SET GLOBAL innodb_file_format = BARRACUDA;DROP USER 'root'@'localhost';CREATE USER 'root'@'%' IDENTIFIED BY 'lge123';GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;FLUSH PRIVILEGES;" && \
mysqladmin -u"root" -plge123 shutdown && \
service mysql start && \
npm run initDb --prefix ./maple-cms-db && \
pm2 start \
./maple-cms-api/maple-cms-api.js \
./maple-cms-api-gateway/maple-cms-api-gateway.js \
./maple-cms-content/maple-cms-content.js \
./maple-cms-display/maple-cms-display.js \
./maple-cms-frontend/server.js && \
/bin/bash
```

### dockerfile에서 파일 내부 수정하기
```dockerfile
### 추가하기
RUN   echo "Some line to add to a file" >> /etc/sysctl.conf

### 수정하기
RUN   sed -i "s|some-original-string|the-new-string |g" /etc/sysctl.conf
```

### docker container에서 pm2 실행 시
```dockerfile
### ENTRYPOINT 설정: docker container에서 pm2를 실행시킬 때는 pm2-docker를 통해서 실행
ENTRYPOINT [ "pm2-docker" ]
### CMD 설정: parameter가 없는 경우, default 값을 지정
CMD ["pm2/dev-system.json"]

```

### docker 내 mongodb service가 restart되지 않는 경우 또는 mongodb가 service에 등록되지 않는 상태에서 restart를 원할 경우
```bash
### mongodb 종료
mongod -f /etc/mongod.conf --shutdown
### mongodb 시작
mongod --fork --config /etc/mongod.conf
```

### docker 내 mysql service가 restart되지 않는 경우
```bash
### mysql service 종료
mysqladmin -u"id" -pPassword shutdown
### mysql service 시작
service mysql start
```

### docker container 접속
```bash
### 실행된 image의 container ID(c456623003b1) 릂 통한 접속
docker exec -it c456623003b1 /bin/bash
### 또는 Dockerfile 내 CMD의 마지막에 /bin/bash 실행
```

### docker volume의 사용방법과 차이점
```bash
https://darkrasid.github.io/docker/container/volume/2017/05/10/docker-volumes.html
```

### docker 명령어들
```bash
### 모든 Container 제거
docker rm -vf $(docker ps -a -q)

### 모든 imnage 제거
docker rmi -f $(docker images -a -q)
```

### 참조 링크

[Dockerfile의 자주 쓰는 instruction들](https://rampart81.github.io/post/dockerfile_instructions/)

[nodejs-pm2-git을 이용한 dockerfile 구성](https://netframework.tistory.com/entry/nodejs-pm2-git%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-Dockfile-%EA%B5%AC%EC%84%B1)

[How to stop mongoDB in one command](https://stackoverflow.com/questions/11774887/how-to-stop-mongo-db-in-one-command/11777141)

[Docker Volume의 사용방법과 차이점](https://darkrasid.github.io/docker/container/volume/2017/05/10/docker-volumes.html)

[Dockerfile 및 Docker-compose를 이용한 세팅 예시](https://gompro.postype.com/post/1735800)

[Dockerfile Entrypoint와 CMD의 올바른 사용 방법](https://bluese05.tistory.com/77)

[Docker 명령어 관련 (정리 필요)](http://pyrasis.com/Docker/Docker-HOWTO)

[Docker Container 연결하기 (정리 필요)](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter06/02)

[Docker directory 연결하기 (정리 필요)](https://dololak.tistory.com/403)

[Docker compose를 활용한 개발 (정리 필요)](https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose)
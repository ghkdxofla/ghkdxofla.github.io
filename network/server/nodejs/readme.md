# pm2

설치
```
npm install pm2 -g
pm2 version
```

Node.js Server를 pm2로 실행
```
# 다음과 같이 할 경우, example이란 이름으로 app이 실행된다
pm2 start app.js --name "example"
```

정지
```
pm2 stop <id|name>
```

목록 보기
```
# 전체 리스트 보기
pm2 list
# 상세 내역 확인
pm2 show <id|name>
```

재부팅
```
pm2 restart <id|name>
# 전체 재부팅
pm2 restart all
```

로그 확인
```
pm2 logs <id|name>
```

모니터링
```
pm2 monit
```

# 참조 링크
[PM2로 프로세스 관리](https://blog.outsider.ne.kr/1197)
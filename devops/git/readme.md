# Password 저장
```bash
git config --global credential.helper store
# 한 번 git pull을 해주면서 비밀번호를 입력하면 저장된다
git pull
```

# Git 에러 warning: LF will be replaced by CRLF 해결방법
```bash
# core.autocrlf 기능 꺼주기
git config --global core.autocrlf false
```
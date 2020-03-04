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

# 특정 폴더나 파일 무시하기(.GITIGNORE)
```bash
# .git 폴더가 있는 동일 위치에 생성
touch .gitignore

# 파일 내용은 다음과 같다

# 파일 무시
test.txt

# 확장자 무시
*.txt

# 폴더 무시
test/

# 기존에 이미 push된 내용 중, 제외 항목이 있을 경우
git rm --cached test.txt
git rm --cached *.txt
git rm --cached test/ -r
git rm --cached . # 전부 지우고 싶을 때

# 또는
git update-index --assume-unchanged {file_name}

# 이후 commit을 해 주어야 파일 삭제가 정상으로 이루어진다
git add .
git commit -m "Remove ingnored file"
```

# 참조 링크
[Git ignore-1](https://kcmschool.com/194)
[Git ignore-2](https://gmlwjd9405.github.io/2017/10/06/make-gitignore-file.html)
# Bash Shell 예시
```bash
# For loop
for VARIABLE in 1 2 3 4 5 .. N
do
    command1 ${VARIABLE}
    command2 ${VARIABLE}
    commandN ${VARIABLE}
done
# in 뒤에 조건문에는 $(COMMAND) 구문으로 명령어 실행 결과를 넣을 수 있음

# 1부터 5까지 2 간격으로
for i in {1..5..2}
do
   echo "Welcome $i times"
done

# 파라미터의 갯수(argc) 는 $# 변수에 담기며 개별 파라미터(argv)는 $숫자 에 저장됨
# $0 은 프로그램 이름(쉘 스크립트명)이 저장되며 $1 부터 파라미터가 저장
echo "Total Param= $#,  PROG: $0, param1 = $1, param2 = $2";

# -eq, -lt 등의 연산자는 integer 비교만 가능하고 문자열 비교는 ==, != 사용
if [ "$a" == "OK" ];then
    //doit
fi

# [  ] 안에 test 조건을 넣을 수 있음
if [ $var -eq 0 ];then
  echo "\$var is 0";
else
  echo "\$var is not 0";
fi

# -d(디렉터리 여부), -f(파일 존재) 등 파일 조건 검사 가능
# /etc/nginx/sites-available/ 디렉터리가 없으면 생성
if [ ! -d "/etc/nginx/sites-available/" ];then
    mkdir /etc/nginx/sites-available/
fi

# -z(문자열이 empty), -n(문자열이 none empty) 등
# $var 문자열이 공백인지 검사
if [ -z $var ];then
    echo "\$var is empty";
fi

# 문자열 치환(처음 1개)
first="I love Suzy and Mary"
second="Sara"
first=${first/Suzy/$second}

# 문자열 치환(전체)
first="Suzy, Suzy, Suzy"
second="Sara"
first=${first//Suzy/$second}
# first is now "Sara, Sara, Sara"
```

# 참조 링크
[Bash shell script programming](https://www.lesstif.com/pages/viewpage.action?pageId=26083916)
[Bash 입문자를 위한 핵심 요약 정리](https://blog.gaerae.com/2015/01/bash-hello-world.html)
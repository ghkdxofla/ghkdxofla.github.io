# Abstract Class

필요 Module을 Import
```python
import abc

# 사용성을 위해 다음의 코드가 추천됨
from abc import ABC, abstractmethod, ABCMeta
```
Abstract Class 구현
```python
class AbstractClassExample(ABC):

    __metaclass__ = ABCMeta
 
    def __init__(self, value):
        self.value = value
        super().__init__()
    
    @abstractmethod
    def do_something(self):
        pass
```
구현된 Abstract Class 사용
```python
class ClassExample(AbstractClassExample):
    def __init(self):
        super().__init__()

    # Abstract Method를 구현하지 않으면 Error 발생
    def do_something(self):
        print("something")

    # Method Overloading 개념도 가능
    def do_something(self, data):
        print(data)
```
# 정적 타입
```python
def typing_example(param_1: int, param_2: str="default") -> str:
    return "example"
```
# String Formatting
```python
## 전체 자리수=8, 소수점은 2자리까지 반올림
print("num: {:8.2f}".format(num))

## 왼쪽 정렬 및 빈 공간 0으로 채우기 
print("num: {:0<8.2f}".format(num))

## 오른쪽 정렬 및 빈 공간 0으로 채우기 
print("num: {:0>8.2f}".format(num))

## 오른쪽 정렬 및 빈 공간 *로 채우기 
print("num: {:*>8.2f}".format(num))
print("="*20)

s = "Taelim Hwang"
print(s)
print("{:|>30s}".format(s))
print("{:|<30s}".format(s))
```

# Hash 사용하기

내장함수 hash의 경우, 3.4 버전 이상부터 Process에 따라 다른 Hash Value가 return 되므로 사용 지양
```bash
python -c "print(hash('123'))"
8180009514858937698
python -c "print(hash('123'))"
-3358196339336188655
python -c "print(hash('123'))"
-5852881486981464238
```
hashlib 사용
```python
import hashlib

# sha256, sha1, md5 등 hash algorithm 선택
# input value는 encode()를 통해 unicode로 변환되어야 한다
# hexdigest()로 16진수의 hex값으로 hash value 출력
hsahlib.sha256(input_value.encode()).hexdigest()
```

# String split 시, 뒤의 1개만 분리하고 싶을 경우
```python
"a b c,d,e,f".rsplit(',', 1)

# Return value
['a b c,d,e', 'f']
```

# Function을 argument로 전달하면서 해당 function의 argument 또한 넘기고 싶을 경우
```python
def a(x, y):
    print x, y

def b(other, function, *args, **kwargs):
    function(*args, **kwargs)
    print other

b('world', a, 'hello', 'dude')

# Return value : 
hello dude
world
```

# Class 및 Function name 확인하기
```python
def my_function():
    pass

class MyClass(object):
    def method(self):
        pass

print(my_function.__name__)         # gives "my_function"
print(MyClass.method.__name__)      # gives "method"

print(my_function.__qualname__)     # gives "my_function"
print(MyClass.method.__qualname__)  # gives "MyClass.method"
```
# Logging

기본 사용법
```
내용 추가
```

추가로 표시하고 싶은 값이 있을 경우
```python
FORMAT = '%(asctime)-15s %(clientip)s %(user)-8s %(message)s'
logging.basicConfig(format=FORMAT)
d = {'clientip': '192.168.0.1', 'user': 'fbloggs'}
logger = logging.getLogger('tcpserver')
logger.warning('Protocol problem: %s', 'connection reset', extra=d)
```

# Coroutine(asyncio)
```
https://iscinumpy.gitlab.io/post/a-simple-introduction-to-asyncio/
```

# Integer to String 시 앞부분 0으로 채우기
```python
str(1).zfill(2) # '01'
```

# 프로젝트의 구조
내용 추가 필요

# 파이썬이 느린 이유
http://jakevdp.github.io/blog/2014/05/09/why-python-is-slow/

# list1에 있는 요소가 다른 list2에 있는지 확인하기
```python
# List of string 
list1 = ['Hi' ,  'hello', 'at', 'this', 'there', 'from']
 
# List of string
list2 = ['there' , 'hello', 'Hi']

# check if list1 contains all elements in list2
result =  all(elem in list1  for elem in list2)
 
if result:
    print("Yes, list1 contains all elements in list2")    
else :
    print("No, list1 does not contains all elements in list2"

# check if list1 contains any elements of list2
result =  any(elem in list1  for elem in list2)
 
if result:
    print("Yes, list1 contains any elements of list2")    
else :
    print("No, list1 contains any elements of list2")
```

# 참조 링크
[Python ABC(Abstract Base Class)](https://bluese05.tistory.com/61)
[Abstract Classes](https://www.python-course.eu/python3_abstract_classes.php)
[정적 타입 선언](http://blog.naver.com/passion053/221070020739)
[Python 출력 시 formatting](https://frhyme.github.io/python-basic/python_string_format/)
[파이썬 프로젝트의 구조](https://www.holaxprogramming.com/2017/06/28/python-project-structures/)
[프로세스에 따른 hash 결과가 다르다](https://charsyam.wordpress.com/2018/01/29/%EC%9E%85-%EA%B0%9C%EB%B0%9C-python-3-3-%EB%B6%80%ED%84%B0%EB%8A%94-hash-%EA%B2%B0%EA%B3%BC%EA%B0%80-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%A7%88%EB%8B%A4-%EB%8B%AC%EB%9D%BC%EC%9A%94/)
[Logging](https://docs.python.org/ko/3/library/logging.html)
[Get class and function name](https://stackoverflow.com/questions/251464/how-to-get-a-function-name-as-a-string)
[Asyncio](https://iscinumpy.gitlab.io/post/a-simple-introduction-to-asyncio/)
[list a에 있는 요소가 다른 list b에 있는지 확인하기](https://thispointer.com/python-check-if-a-list-contains-all-the-elements-of-another-list/)
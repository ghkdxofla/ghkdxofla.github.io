# Abstract Class

필요 Module을 Import
```
import abc

# 사용성을 위해 다음의 코드가 추천됨
from abc import ABC, abstractmethod, ABCMeta
```
Abstract Class 구현
```
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
```
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
```
def typing_example(param_1: int, param_2: str="default") -> str:
    return "example"
```
# String Formatting
```
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

# 프로젝트의 구조
내용 추가 필요

# 참조 링크
[Python ABC(Abstract Base Class)](https://bluese05.tistory.com/61)
[Abstract Classes](https://www.python-course.eu/python3_abstract_classes.php)
[정적 타입 선언](http://blog.naver.com/passion053/221070020739)
[Python 출력 시 formatting](https://frhyme.github.io/python-basic/python_string_format/)
[파이썬 프로젝트의 구조](https://www.holaxprogramming.com/2017/06/28/python-project-structures/)

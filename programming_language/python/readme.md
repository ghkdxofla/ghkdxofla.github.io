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
참조 링크
- [Python ABC(Abstract Base Class)](https://bluese05.tistory.com/61)
- [Abstract Classes](https://www.python-course.eu/python3_abstract_classes.php)
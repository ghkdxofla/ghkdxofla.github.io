---
layout: post
title:  "final, static 그리고 final static. 아, Immutable Object"
categories: [Programming Language, Java]
shortinfo: "Java를 종종 쓸 때마다 꼭 다시 검색해보는 것들이다. 알긴 아는데, 잘 아는게 아닌듯 해서 정리해본다. 하나 더, 불변 객체도."
tags: [Programming Language, Java, static, final, Immutable Object]
comments: true
---

# final?

사실, Java 외에도 final keyword는 많은 언어에 존재한다.   
주로는 상수, 메소드, 클래스 정의 후 `고정된 상태`를 만들기 위해 사용한다.   
고정된 상태라 하면, 불변하는 것이라 생각할 수 있다.   
그렇다면 final로 생성한 변수가 불변인가?   
그런데, 불변하다는 것이 정확히 무엇인가?   

# Immutable?

## 정의

Immutable Object는 생성 후 상태를 변경하지 못하는 객체를 일컫는다.   
굳이 객체 외에도 Immutable 하다는 것은 한 번 정의된 대상이 상태 변경이 불가능 함을 의미한다.   

> 한 번 할당하면 변경 안함!
> 재할당은 가능!

## 왜 쓰는가?

Immutable Object를 사용하면 복제, 비교 시에 조작이 단순해진다.   
성능 개선에도 영향을 준다.   
물론, 하나의 객체를 재사용하며 변경이 자주 일어날 경우에는   
Mutable Object가 더 유용하다.   

이 외에도 Object에 대한 장, 단점은 다음과 같다.   

### 장점

- 객체에 대한 신뢰도가 높아진다. 객체가 한번 생성되어서 그게 변하지 않는다면 transaction 내에서 그 객체가 변하지 않기 때문.
- 생성자, 접근메소드에 대한 방어 복사가 필요없다.
- 멀티스레드 환경에서 동기화 처리없이 객체를 공유할 수 있다. 

### 단점

- 객체가 가지는 값마다 새로운 객체가 필요하다.
- 메모리 누수와 새로운 객체를 계속 생성해야하기 때문에 성능저하를 발생시킬 수 있다.

**참고** : [[Java] Immutable Object(불변객체)](https://velog.io/@conatuseus/Java-Immutable-Object%EB%B6%88%EB%B3%80%EA%B0%9D%EC%B2%B4)

## 어떤 것이 있는가?

언어들 전반적으로는 원시 타입(Primitive type) 이 Immutable 하다.   
Immutable Object는 보통 getter는 있으나 setter가 없게끔 만들면 가능하다.   
위에서 참고한 블로그의 예시를 하나 보자면,

```java
import java.util.Collections;
import java.util.List;

public class ListObject {

    private final List<ImmutableObject> immutableList;

    public ListObject(final List<ImmutableObject> immutableList) {
        this.immutableList = new ArrayList<>(immutableList);
    }

    public List<ImmutableObject> getImmutableList() {
        return Collections.unmodifiableList(immutableList);
    }
}
```

이렇게 값 추가/삭제가 불가능하도록 Collection의 unmodifiableList 메서드를 사용하여 getter를 만들었다.   

# 다시, final로...

final을 사용하면 불변 상태가 된다고 했는데   
위의 Immutable에서 설명한 부분을 참고하면   
final이 immutable한 것은 아니라는 것을 알 수 있다.   

> 무슨 말이고...

`고정된 상태`라 함은 final로 정의된 상수나 메소드가   
한 번 할당된 후 다시 재할당이 안되는 것을 의미한다.   
각 요소에서 final을 사용할 경우 다음의 효과를 갖는다.   
- 상수의 경우, 재할당이 안되도록 막는다.   
- 메소드의 경우에는 오버라이딩이 안되도록 막는다.   
- 클래스는 상속하지 못하도록 한다.   

분명, `재할당`이 안된다고 했지, 상태에 대한 변경이 안된다는 부분은 없다.   
즉, mutable object를 final 키워드로 정의하면   
속성이나 데이터에 대한 변경은 가능하나 재할당이 안되는   
변수를 정의할 수 있는 것이다.   

**참고** : [[Java] final 과 static final 차이](https://m.blog.naver.com/PostView.nhn?blogId=goddlaek&logNo=220889229659&proxyReferer=https:%2F%2Fwww.google.com%2F)

# static?

static은 Java에서 클래스(정적) 멤버를 설정할 때 사용하는 키워드이다.   
JVM에 class loading을 할 때 클래스의 멤버, 메소드 등이 JVM의 class 영역에 올라간다.   
이 때, static 키워드를 사용한 멤버는 해당 class 영역에 올라가 있으므로   
별도의 인스턴스 생성 없이 사용 가능하다.   

## 장점

- 객체 생성 없이도 접근 및 사용이 가능하다.   

## 단점

- Garbage collector 밖에 위치하여 메모리에 상주한다. 시간이 지나면 시스템 속도 저하의 원인이 될 수 있다.
- static 내부에서 외부로의 접근이 불가능하다. 접근하려면 같은 static 키워드로 정의된 요소만 가능하다.

**참고** : [[Java]static](https://blog.naver.com/goddlaek/220888359923)

# static final?

단순하다.   
static + final이다.   
클래스 멤버 변수이면서, 최초 선언과 동시에 초기화가 되어야 하며,   
재할당이 안되도록 하는 클래스 상수로 보면 된다.   

> 끝!
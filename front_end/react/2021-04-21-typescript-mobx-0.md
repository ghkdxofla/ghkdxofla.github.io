---
layout: post
title:  "Typescript로 개발하는 React with MobX (0)"
categories: [Typescript, React, MobX, Front-end]
shortinfo: "프론트엔드 개발은 리액트로 하면 와- 재.밌.겠.다."
tags: [Typescript, React, MobX, Front-end]
comments: true
---

# 프론트엔드를 개발해 보겠습니다

UI... 사용자에게 보여지는 화면... Server와 Client에서 Client를 맡고 있는... 프론트...엔드...   
여러 라이브러리와 프레임워크, 언어로 앱 또는 웹 어플리케이션을 만들 때 사용자에게 보여지는 GUI 부분을 우리는 `Front-end`라 칭한다.   
다음의 [블로그](https://dreamaz.tistory.com/27)에서 참조하면 아래와 같은 설명을 확인할 수 있다.   

> 프론트엔드와 백엔드는 프로그램 인터페이스와 서비스의 최초 사용자와 관련된 특성을 나타내는데 사용되는 용어이다 (여기서 "사용자"란 사람 또는 프로그램이 될 수 있다). 프론트엔드 응용프로그램은 사용자들과 직접 상호작용을 하는 프로그램이다.

아무튼, 이번에는 React를 써서 Front-end를 개발해보겠다.   
언어는 Typescript.   
그리고 뭔지 아직은 모르는 MobX를 쓸 것이다.   
그 결과물은 웹서비스 페이지가 된다!   

# React?

`React`는 페이스북에서 개발한 Front-end library다.   
`SPA(Single Page Application)` 로 개발할 때 쓰는 대표적 library다.   
SPA에 대한 내용은 다음의 [블로그](https://linked2ev.github.io/devlog/2018/08/01/WEB-What-is-SPA/)를 참고한다.   
간단하게는, 브라우저에 최초에 한번 페이지 전체를 로드하고, 이후부터는 특정 부분의 데이터를 변경하는 방식이다.   
더 간단하게는, 화면 전환 시, 새로 Rendering 되면서 생기는 번쩍임이 없다.   
주요 특징으로는 다음과 같다.   

1. **Component**   
   React는 `Component`로 구성되어 있다.   
   UI의 각 요소가 Component 단위로 개발되고, Component 여러 개가 모여 더 큰 Component를 구성한다.   
   이는 높은 `재사용성`을 가지게 되어 효율적인 개발이 가능하다.   
   Component는 Class형(Stateful)과 Function형(Stateless)로 나뉜다.   
   최근에는 Function형으로 개발을 많이 진행하는 것으로 보인다.   

2. **단방향 데이터 흐름**   
   데이터가 `한 방향`으로 흐른다.   
   즉, 부모 컴포넌트에서 자식 컴포넌트로 데이터가 내려간다(`Props`).   
   문제는, 개발할 때 자식 -> 부모 방향으로 데이터 변경이 필요할 때가 존재한다.   
   이럴 때 `State`의 변경을 통해 상태 변경으로 데이터를 변경한다.   

3. **Virtual DOM**   
   일반적으로 HTML 기반의 웹페이지는 `DOM(Document Object Model)`으로 표현된다.   
   DOM은 브라우저에서 HTML 문서를 Parsing하여 객체로 구조화하는 것을 의미한다.   
   이 DOM에 대해서는 다음에 더 자세히 다루기로 하고, 참조할 링크만 남겨둔다.   

   ---   
   [브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)   
   [React 적용 가이드 - React 작동 방법](https://d2.naver.com/helloworld/9297403)   
   [Virtual DOM 동작 원리와 이해 (with 브라우저의 렌더링 과정)](https://jeong-pro.tistory.com/210)   
   
   ---

   간단하게 정리하면, DOM을 Rendering 해야 브라우저의 화면이 보이는데,   
   HTML에서 일부 값이 변경되면 계속하여 전체 DOM을 Re-rendering 해야한다.   
   매우 비효율적이며, 반응도 느리게 된다.   
   이를 위해 React는 가상의 DOM을 만들어 실제 DOM과 비교 후,   
   변경 사항이 있을 경우에만 DOM에 변경 부분을 반영한다.   

React도 더 얘기하면 한 세월이니, 여기까지만 하겠다.   

# Typescript?

Javascript에 Type을 추가한 언어이다.   
Javascript의 확장(Superset)이다.   
정적 타입을 지원하여 컴파일 단계에서 오류를 체크할 수 있는 장점이 있다.   
그리고 Typescript로 개발하는 React 내용은 다음 포스트에서 중점적으로 다룰 예정이다(`귀찮음`).   
나머지 내용은 [블로그](https://velog.io/@taeg92/TypeScript-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0) 참조.   

# MobX?

`MobX`는 React의 상태를 관리해주는 library이다.   
상태를 전역으로 관리하는 것이 큰 특징이다.   
위에서 잠깐 언급한 State에 대해 관리해주는 library는 여러 가지가 있지만   
대표적으로 `Redux`와 `MobX`를 많이 사용한다.   
주의할 점은, MobX가 **React에 종속된 library**가 아니라는 점이다.   

## 상태 관리?

위에서 언급한 Props, State에서 State를 관리하는 것을 의미한다.   
State의 변경에 따라 React는 UI를 업데이트 하는데, MobX는 이러한 State를   
전역으로 관리하게 함으로써 `유지보수의 간편함`을 가져왔다.   

성능상으로도, MobX가 `필요한 경우에만` State를 변경하게 되므로,   
React에서 Virtual DOM의 변경 및 적용이 최소화된다.   
이는 더 빠른 웹 어플리케이션 개발을 의미한다.   

> Anything that can be derived from the application state, should be derived. Automatically.

이를 도식화하면 다음과 같다.   
![Concept of MobX](/assets/media/20210421_typescript_mobx_0_1.png)   

## 주요 개념

위의 도식에 대한 각 부분의 개념 및 설명은 다음과 같다.   

### State

Observable state이다.   
상태에 대한 관찰이 이루어지며, 변경 시, `Reactions`, `Computations`을 발생시킨다.   

### Derivations(Computed values, Computations)

Observable state의 변화에 따른 파생 값이다.   
성능 최적화를 위해 사용한다.   

### Reactions

Observable state의 변화에 따른 추가적인 변화이다.   
값이 생성되지 않는 함수의 개념이다.   
DOM의 업데이트 또는 네트워크 요청을 하여 I/O 와 관련된 작업을 수행한다.   

### Actions

Observable state에 대한 변경을 일으키는 모든 행위이다.   

## Redux VS MobX

주로 이 두 개의 library를 전역 상태 관리에 사용하는데,   
차이점은 다음과 같다.   

### Redux
- Immutable 유지가 중요
- [Flux Architecture](https://facebook.github.io/flux/)를 따름
- Single Store
- Middleware
- 함수형 프로그래밍
- Dispatch 관리를 위해 redux-saga, redux-thunk 등의 추가 middleware가 필요함
- action, reducer, dispatch 등 사용

### MobX
- Immutable 유지에 대해 신경쓰지 않아도 됨
- OOP
- Store를 여러 개 사용 가능
- Decorator 사용

이 외의 자세한 내용은 [블로그](https://jeffgukang.github.io/react-native-tutorial/docs/state-tutorial/mobx-tutorial/02-what-is-mobx/what-is-mobx-kr.html) 참조.   

# 개발은 언제 할건데

이미 개발을 어느정도 진행하였다!   
그래서 각 요소(React, Typescript, Mobx)에 관한 내용을   
다음 포스트에 각각 상세히 설명할 예정이다.   
설명 순서는 중구난방이 될 것으로 보인다.   

# 참조

[React란?](https://velog.io/@stampid/React%EB%9E%80)   
[Mobx란?](https://jeffgukang.github.io/react-native-tutorial/docs/state-tutorial/mobx-tutorial/02-what-is-mobx/what-is-mobx-kr.html)   
[MobX 란? MobX 와 React](https://byseop.netlify.app/hello-mobx/)
[https://velog.io/@velopert/begin-mobx](https://velog.io/@velopert/begin-mobx)
---
layout: post
title:  "Typescript로 개발하는 React with MobX (1)"
categories: [Typescript, React, MobX, Front-end]
shortinfo: "프론트엔드 개발은 리액트로 하면 와- 재.밌.겠.다."
tags: [Typescript, React, MobX, Front-end]
comments: true
---

# 글 제목은 Typescript인데...

천천히 글을 연재하면서 내용을 추가할 예정이다.   
아무튼 내 맘대로 React 얘기도 했다가, MobX 얘기도 했다가,   
Typescript 얘기도 할 것이니 문제 없음.   

# 프로젝트 구조

항상 새로운 프로젝트를 시작할 때면,   
어떻게 프로젝트 구조를 잡아야 할 지가 큰 고민일 때가 많다.   
이럴 때는 다른 프로젝트 구조를 참고하기도 하고,   
사용하는 라이브러리나 프레임워크에서 제공하는   
템플릿을 활용할 때가 많다.   

현재 개발 중인 프로젝트의 구조 또한   
지인이 개발하던 프로젝트의 구조를 많이 참고하여   
내 입맛에 맞게 변형한 것을 사용한다.   
즉, 나 또는 같이 개발하기 편하고,   
구조적으로 명확한 단위로 나뉜다면   
어떻게 구조를 잡든 합리적인 구조라 볼 수 있다.   

다음은 참고를 위한 프로젝트 구조의 예시이다.   

![프로젝트 구조](/assets/media/20210422_typescript_mobx_1_0.png)

살펴볼 부분은 src 폴더 내부의 구조이다.   

```javascript
src
|---assets // static, media file 관리
|---config // 공용 설정 파일 관리
|---components // React 컴포넌트 관리(Dumb component)
|---containers // App의 상태 제어가 포함된 컴포넌트 관리(Smart component)
|---hooks // Custom hook 관리
|---models // 데이터 모델 관리
|---pages // Router 단위의 페이지 관리
|---repositories // API 호출 관리
|---stores // MobX의 Store 관리
|---utils // Custom util 관리
```

이렇게 프로젝트 구조를 단위 별로 잘 묶어놓으면 다음의 장점이 있다.   
- 비즈니스 로직 구현 부분과 View Only 부분의 구분이 용이함
- 컴포넌트의 재사용성 증가
- 협업 시 개발 단위 분리의 편리함

시간이 지난 뒤에도 다시 개발을 한다 치면,   
어떤 개발자든 다시 보면서 생각해야 하는 시간이 필요하다.   
이렇게 명시적으로 분리해 놓으면 그러한 시간이 크게 감소한다.   

가령, 아무 비즈니스 로직이 없는 컴포넌트에 대해 수정을 한다고 하자.   
그럴 경우에는 바로 components 폴더 내부에서 수정할 컴포넌트를 변경하면 된다.   
비즈니스 로직이 포함된 Controller 부분 개발이 필요하다.   
그러면 containers, repositories, stores 등 관련 부분 위주로 참고하면 된다.   

> 프로젝트 구조는 개발 편의성과 재사용성을 중심으로 고민!

참고할만한 블로그를 적고, 다음 주제로 넘어간다.   

---
[Container와 Component](https://www.zerocho.com/category/React/post/57e1428c11a9b10015e803aa)   
[5단계로 보는 리액트 폴더구조](https://smoh.tistory.com/385)   

---

#  MobX 사용하기

사실, Typescript 기반의 React는 Javascript 기반으로 할 때 보다   
어떤 점이 좋은지를 먼저 얘기할까 했는데,   
언어적 차이보다 더 중요한 것 부터 설명하고자 한다.   

앞선 포스트에서 MobX에 대해 간단히 알아봤다면,   
위의 프로젝트 구조를 토대로 MobX를 어떻게 활용하는지   
코드 기반으로 내용을 채워보았다.   

## Pre-installation

보통 React는 `create-react-app` 을 통해 초기 프로젝트 구성을 세팅한다.   
MobX 설치 전에는 React의 프로젝트 설정과 스크립트를 수정 가능하도록 변경해야 한다.   
이유는, MobX에서 사용하는 decorator 문법을 사용하기 위함이다.   
기본적으로 create-react-app 으로 만든 프로젝트에서는 decorator 문법이 작동하지 않는다.   

```bash
yarn run eject
```

또는, `react-app-rewired` 라는 것을 사용한다는데, 참조한 [블로그](https://medium.com/@jsh901220/mobx-%EC%B2%98%EC%9D%8C-%EC%8B%9C%EC%9E%91%ED%95%B4%EB%B3%B4%EA%B8%B0-a768f4aaa73e)를 다시 참조해보자(귀찮음).   

추가로, `babel-preset-mobx` 설치를 통해 제대로 decorator를 사용할 수 있게 된다.   

```bash
yarn add babel-preset-mobx
```

설치 후, `package.json` 에 다음을 추가한다.   

```json
...
"babel": {
  "presets": [
    "react-app",
    "mobx"
  ]
}
...
```

## Installation

```bash
yarn add mobx mobx-react
```

## 주요 기능

### Observable state

#### **observer**

`mobx.autorun` 라는 함수로 컴포넌트를 감싸는 구조로 사용한다.   
`observable` 한 데이터가 변경되면 `observer` 가 선언된 컴포넌트는 re-rendering 된다.   
observer는 MobX가 최적화 해주므로, 모든 컴포넌트에 사용하면 좋다고 한다.   

아까 참고했던 [블로그](https://medium.com/@jsh901220/mobx-%EC%B2%98%EC%9D%8C-%EC%8B%9C%EC%9E%91%ED%95%B4%EB%B3%B4%EA%B8%B0-a768f4aaa73e)에서는 decorator를 써서 observer를 사용했다.   
단, Class 기반으로 React를 구성한 것도 있고,   
초기에 decorator가 잘 작동하지 않아 본인은 다음과 같이 사용하였다.   

```typescript
...
import { observer } from "mobx-react";
...

const Container = (props: object) => {
  ...

  return (
    <>
    ...
    </>
  );
};

export default observer(Container); // 이 부분!
```

Container 함수 자체를 마지막에 export 할 때 observer로 감싸서   
observable한 데이터를 인지할 수 있도록 하였다.   

또 하나 중요한 점은, 본인의 경우에는 containers 폴더 하위의   
`Container 컴포넌트`에만 observer를 사용하였다는 점이다.   
나머지 dumb component(UI 위주로만 개발된 컴포넌트)에서 observable을 사용하면   
결국 모든 state 변경에 대응할 수 있게 된다.   

#### **observable**

state를 추적할 데이터에 observable decorator를 사용한다.   
MobX가 6 버전 이후로는 `makeObservable` 함수를 사용하는 쪽으로 권장한다.   
본인은 애매하게 둘 다 사용한 코드로 개발을 하고야 말았다...   
우선은 개발 기준으로 refactoring 전 단계의 코드를 첨부한다.   
decorator 안 쓰는 방법은 다음의 [블로그](https://sunnykim91.tistory.com/138) 참조

```typescript
...
class Store {
  @observable userData: UserModel | null = null; // Observable state 설정
  @observable loginStatus: boolean = false;
  @observable loginStatusReason: number = 0;
  @observable isLoading = {
    fetchUserData: false,
    dropUserData: false
  }

  fetchUserData: (userId: string, userPw: string) => CancellablePromise<void>;
  dropUserData: (userId: string) => CancellablePromise<void>;

  constructor() {
    makeObservable(this); // MobX 6 버전 이후로는 decorator 말고 이 함수로 observable을 사용하는 것으로 변경됨

    this.fetchUserData = flow(this._fetchUserData.bind(this)); // 이 부분은 다음에 설명
    this.dropUserData = flow(this._dropUserData.bind(this));
  }

  @action.bound init() { // MobX에서 데이터 조작을 통해 상태 변경을 할 때 action을 사용함
     ...
  }

  *_fetchUserData(userId: string, userPw: string) { // 이 부분은 다음에 설명
    if (this.isLoading.fetchUserData) return;
    this.isLoading.fetchUserData = true;
    try{
    const result = yield AuthRepository.getUserData(userId, userPw);
      if(result && result.user?.userID){
        this.userData = new UserModel(result);
        this.loginStatus = true;
      }
      else{
        this.loginStatus = false;
        this.loginStatusReason = result ? result.reason : 5;
      }
    }catch(e){
      console.log(e);
    }
    this.isLoading.fetchUserData = false;
  }
  ...
...
```

추가적인 내용은 다음의 포스트에서 이어나가도록 한다.   
  
# 참조

[React MobX 초심으로 돌아가서 공부하기(MobX최신 버전반영/MobX6)](https://sunnykim91.tistory.com/138)   
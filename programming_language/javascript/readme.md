# 자바스크립트 비동기 처리와 콜백 함수

Promise? 비동기?
- Promise는 자바스크립트 비동기 처리에 사용되는 객체
- 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행

비동기 코드의 예
```javascript
// #1
console.log('Hello');
// #2
setTimeout(function() {
	console.log('Bye');
}, 3000);
// #3
console.log('Hello Again');

// 결과
‘Hello’ 출력
‘Hello Again’ 출력
3초 있다가 ‘Bye’ 출력
```

Callback?
- 데이터가 준비된 시점에서만 원하는 동작을 수행

콜백 함수를 통한 비동기 처리 해결
```javascript
function getData(callbackFunc) {
	$.get('https://domain.com/products/1', function(response) {
		callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
	});
}

getData(function(tableData) {
	console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

Callback Hell 해결
- Promise나 Async 사용
- Coding Pattern 사용
```javascript
function parseValueDone(id) {
	auth(id, authDone);
}
function authDone(result) {
	display(result, displayDone);
}
function displayDone(text) {
	console.log(text);
}
$.get('url', function(response) {
	parseValue(response, parseValueDone);
});
```

# Exception handling에서 finally 사용하기
```javascript
axios.get('/user')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .then(function () {
	// always executed
	// this means it can be 'finally' function
  }); 
```

# axios

Axios란?

# 내용 추가 필요
https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
https://hyunseob.github.io/2016/03/10/javascript-this/

# forEach에 break 말고 some 사용하기

왜?
- javascript는 break 문이 존재하지 않는다

break가 없으면?
- 불필요한 순회를 없애고 싶을 때 문제 발생

대안은?
- 커스텀 오류 객체 사용 : 코드 품질 저하
- some 사용 : some은 컬렉션의 요소 중 최소 1개라도 조건을 만족시키는지 검사하는 Method

예제
```javascript
// 예시 : 2에서 탈출하고 싶다면...?
[1,2,3,4,5].forEach(function(v) {
  if (v==2) console.log(v)
});

// 커스텀 오류 객체 사용 예시
// 코드가 지저분하다...!
var Break = new Error('Break');

try {
  [1,2,3,4,5].forEach(function(v) {
    if (v==2) {
      console.log('v: ', v)
      throw Break;
    }
  });
} catch (e) {
  if (e!= Break) throw Break; 
}

// some 사용
// 가독성도 높고 훨신 깔끔하다
[1,2,3,4,5].some(function(v) {
   if(v == 2) console.log(v);
   return (v ==2);
});
```

# 리스트 내부의 값이 존재하는지 확인이 필요할 때
```javascript
// 리스트 내부 1 depth 값의 경우
arr.includes('apple');


// 리스트 내부 객체의 경우
arr.find(el => el[0] === color);


```
# 참조 링크
[자바스크립트 비동기 처리와 콜백 함수](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/#%EC%BD%9C%EB%B0%B1-%EC%A7%80%EC%98%A5-callback-hell)

[forEach에 break문 대신 some 사용하기](https://blog.outsider.ne.kr/847)
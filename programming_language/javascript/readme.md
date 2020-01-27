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

# 내용 추가 필요
https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
https://hyunseob.github.io/2016/03/10/javascript-this/

# 참조 링크
[자바스크립트 비동기 처리와 콜백 함수](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/#%EC%BD%9C%EB%B0%B1-%EC%A7%80%EC%98%A5-callback-hell)
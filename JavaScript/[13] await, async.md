## async/await

---

```jsx
async function fetchAndPrint() {
	const response = await fetch('https://jsonplaceholder.typicode.com/users');
	const result = await response.text();
	console.log(result);
}
```

- async
    - 함수 안에 비동기적으로 실행되는 부분이 있다(await)
- await
    - 뒤에 코드를 실행하고 프로미스 객체가 fulfilled/rejected 될때까지 기다림, fulfilled가 되면 작업 실행 결과 리턴
    - async 안에서만 사용 가능함.

```jsx
async function fetchAndPrint() {
	console.log(2);
	const response = await fetch('https://jsonplaceholder.typicode.com/users');
	// fetch 완료될때까지 4는 실행되지 않음. 함수 밖의 나머지 코드들 실행
	console.log(4);
	const result = await response.text();
	console.log(result);
}

console.log(1);
fetchAndPrint();
console.log(3); // await 만나면 3부터 실행
```

- await이 있으면 보이는대로 실행되는것이 아님!!
- await을 만나게 되면 실행 순서를 함수의 다음 코드로
- 기존 promise 구문을 깔끔하게 작성할 수 있음.

### rejected 상태가 된다면

---

```jsx
async function fetchAndPrint() {
	try {
		const response = await fetch('https://jsonplaceholder.typicode.com/users');
		const result = await response.text();
		console.log(result);
	} catch (error) {
		// rejected 상태가 생기면 catch문으로 코드의 실행 흐름 넘어옴
		// error 파라메터에 작업 실패 정보
	} finally {
		// 잘 성공하든 실패하든 무조건 실행
	}
}

fetchAndPrint();
```

- try .. catch문을 사용하면 된다.

## async

---

- async가 붙은 함수는 항상 Promise 객체를 리턴한다.
- promise 객체가 리턴되면 해당 객체와 동일한 상태와 작업 성공 결과 / 실패 정보 
가진 프로미스 객체 그대로 리턴
- promise 객체가 아닌 숫자나 문자열, 일반 객체 등을 리턴하는 경우
    - fulfill 상태이면서, 리턴값을 작업 성공 결과로 가진 Promise 객체 리턴
    - undefined인 경우도 fulfill 상태이면서, 작업 성공 결과가 undefined인 promise 객체
    - 에러 발생시 rejected 상태이면서 에러 객체를 작업 실패 정보로 가진 promise 객체 리턴

```jsx
async function f() {
	return 3;
} // 숫자 3을 작업 성공 결과로 가진 fulfill 상태의 promise 객체 리턴

async function f() {
	return fetch('https://jsonplaceholder.typicode.com/users')
		.then((response) => response.text());
} 
/*promise 객체가 리턴되면 해당 객체와 동일한 상태와 작업 성공 결과 / 실패 정보 
가진 프로미스 객체 그대로 리턴*/
```

## async 안의 async 함수

---

- async 함수는 promise 객체를 리턴
- 다른 async 함수에서 await 키워드를 붙여서 사용할 수 있다.

## async 위치

---

```jsx
async function f() { }

const f = async function() { }
const f = async (a) => {}
const f = async (a) => a;

( function f() {
	console.log('test')
}()); // 즉시 실행 함수

(async function f() {
	console.log('test')
}());
```

## async 함수 작성시 성능

---

- 요청 하나씩 보내고 기다리는 케이스
    - 순서가 중요한 경우

```jsx
for (const url of urls) {
	const response = await fetch(url);
	console.log(await response.text());
}
```

- 일단 모든 요청을 다 보내고 기다리는 케이스
    - 순서같은게 중요하지 않은 경우

```jsx
for (const url of urls) {
	(async () => {
		const response = await fetch(url);
		console.log(await response.text());
	})();
}
```

- 콜백이 여러번 실행되는 경우는 promisify 불가능
- async/await은 함수 내가 아닌 코드의 최상위 영역에서는 사용 불가능 (then 메서드 써야함.)
- 콜백에는 동기 실행과 비동기 실행 모두 존재한다.

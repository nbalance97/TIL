# 비동기 실행

---

- Start! → end! → fetch 실행

```jsx
console.log('start!')

fetch('https://jsonplaceholder.typicode.com/users')
	.then((response) => response.text())
	.then((result) => {console.log(result);});

console.log('end!')
```

- then 메소드 : 콜백을 등록하는것, 실행하지는 않음.
- 따라서 end가 먼저 실행되고, 서버의 response가 도착하면 콜백이 실행됨.
- fetch → promise 객체 리턴, 비동기 실행과 연관 있음.

### 비동기 실행

---

- 특정 작업을 시작하고 완벽하게 다 처리하기 전에, 실행 흐름이 바로 다음 코드로 넘어가고 나중에 콜백이 실행되는 것
- 동기 실행 → 한번 시작한 작업을 다 처리하고 나서 다음코드로 넘어감

    ### 비동기 실행 함수

    1. setTimeout 함수
        - 특정 함수의 실행을 원하는 시간만큼 뒤로 미루기 위해 사용하는 함수

        ```jsx
        console.log('start!')

        setTimeout(() => { console.log("end1"); }, 2000) // 2초 뒤

        console.log('end2')
        ```

    2. setInterval 함수
        - 특정 콜백을 일정한 시간 간격으로 실행하도록 등록하는 함수

        ```jsx
        console.log('start!')

        setInterval(() => { console.log("end1"); }, 2000) // 2초 간격으로 계속 실행

        console.log('end2')
        ```

    3. addEventListener
        - 특정 조건(클릭 등)이 만족될때 마다 실행

        ```jsx
        btn.addEventListener('click', (e) => {
        	// 처리 내용
        });
        ```

    - fetch 함수는 좀 더 새로운 방식으로 비동기 실행을 지원하는 자바스크립트 문법
        - Promise 객체 리턴
        - Promise 객체 : 비동기 실행을 지원하는 또 다른 종류의 문법

## Promise

---

- 작업에 관한 상태 정보를 갖고 있는 객체
- 작업이 성공했는지 실패했는지 알 수 있음.
    - pending - 작업 진행 중
    - fulfilled - 작업 성공 → 작업의 성공 결과또한 가지고 있음.(fetch의 then의 response)
    - rejected - 작업 실패 → 작업의 실패 이유에 대한 정보또한 가지고 있음.
- then 메소드도 Promise의 메소드

```jsx
fetch('https://jsonplaceholder.typicode.com/users')
	.then((response) => response.text())
	.then((result) => {console.log(result);});
// then은 pending->fulfilled 상태가 되었을때 실행할 코드 등록
// 작업 성공 결과는 콜백의 첫번째 파라메터
```

## Promise Chaining

---

- then메서드 뒤에 then메서드를 붙이는 것
- Promise 객체를 계속해서 연결
- then 메서드가 새로운 Promise 객체를 리턴함.
    - 가장 처음에는 Pending 상태
    - callback에서 리턴하는 값에 따라 달라짐
        1. promise 객체 리턴 ⇒ 콜백에서 리턴한 promise 객체와 같은 상태, 같은 정보
        2. promise가 아닌것 리턴 ⇒ fulfilled 상태, 작업 성공 결과 : return값

```jsx
fetch('https://jsonplaceholder.typicode.com/users')
	.then((response) => response.text()) // text 메서드는 promise 리턴
	.then((result) => {
		return 'a';
	}).then((t) => { console.log(t); });
```

- response.text(), response.json() ⇒ promise 객체 리턴
- 비동기 작업을 순차적으로 실행할 때, 전체 코드를 깔끔하게 하기 위해 사용

## Rejected 상태일때의 콜백

---

- then 메서드의 두번째 인자에 error시의 콜백함수
- 작업 실패 시 작업 실패 정보가 들어옴

```jsx
fetch('https://jsonplaceholder.typicode.com/users')
	.then((response) => response.text(), (error) => { console.log(error); }) 
	.then((result) => {
		return 'a';
	}).then((t) => { console.log(t); });
```

- 콜백 실행 중간에 에러 발생 시
    - Rejected, 작업 실패 정보로 에러 객체를 가짐.

## Callback

---

1. Promise 객체 이외의 값 리턴

    → fulfilled, 작업 성공 결과로 해당 값 가짐

2. 콜백의 리턴이 없음

    → fulfilled, undefined 리턴한 것으로 간주

3. 콜백 내부 에러

    → rejected, 작업 실패 정보로 에러 객체

4. 콜백이 없을때

    → 이전 promise 객체와 동일한 상태와 정보를 가짐.

## Catch

---

- promise rejected시 실행되는 코드 지정(then메서드의 두번째 콜백 안써줌)
- Catch 메서드는 fulfilled 리턴
- then 메서드를 변형한것과 같음.
    - then(undefined, (error) => { console.log(error); })
    - 에러가 발생하더라도 then메서드를 타고 내려간다. 종료 X (에러 객체가)

    ```jsx
    fetch('https://jsonplaceholder.typicode.codddm/users')
    	.then((response) => response.text())
    	.catch((error) => { console.log(error); })
    	.then((result) => {
    		return 'a';
    	}).then((t) => { console.log(t); });
    ```

- catch 메소드는 마지막에 쓰인다.
    - 어디서 에러가 생기던지 간에 가장 잘 대처하는 방법
    - 넘어온 에러는 name과 message 프로퍼티를 통해 살펴볼 수 있다.

    ```jsx
    fetch('https://jsonplaceholder.typicode.codddm/users')
    	.then((response) => response.text())
    	.then((result) => {
    		return 'a';
    	})
    	.then((t) => { console.log(t); })
    	.catch((error) => { console.log(`${error.name}이고, 이유는.. ${error.message}`); });
    ```

- 에러 발생시 대안을 뒤로 넘겨줄 수 있다면 catch를 중간에 써도 됨.
    - 작업을 살릴수 있는 방법이 있다면 Promise Chaining 중간에 catch
    - 예시로 .. A → B → C로 넘겨줄 때, B에서 에러가 발생한 경우 D로 대체할 수 있다면 catch문에서 B를 D로 대체 후 진행.
        - A → D → C

## Finally

---

- 성공하던지, 실패하던지 상관 없이 항상 실행하고 싶은 코드 등록
- catch보다도 뒤에 작성
    - 파라메터는 따로 필요하지 않음

    ```jsx
    fetch('https://jsonplaceholder.typicode.codddm/users')
    	.then((response) => response.text())
    	.then((result) => {
    		return 'a';
    	})
    	.then((t) => { console.log(t); })
    	.catch((error) => { console.log(`${error.name}이고, 이유는.. ${error.message}`); })
    	.finally(() => {console.log('i am finally')});

    fetch('https://jsonplaceholder.typicode.codddm/users')
    	.then((response) => response.text())
    	.then((result) => {
    		return 'a';
    	})
    	.then((t) => { console.log(t); })
    	.catch((error) => { 
    		console.log(`${error.name}이고, 이유는.. ${error.message}`);
    		new Error('error!!'); 
    	})
    	.finally(() => {console.log('i am finally')});
    ```

## Promise 객체의 등장 배경

---

- Promise가 없다면?
    - 콜백 지옥 또는 콜백 헬(callback hell), 지옥의 피라미드

    ```jsx
    fetch(url, function c1() {
    	fetch(url, function c2() {
    		fetch(url, function c3() {
    			...
    		}
    	}
    })
    ```

- 콜백 헬 문제 해결, 비동기 작업 처리에 관한 좀 더 세밀한 처리 가능

## Promise 생성

---

```jsx
const prom = new Promise((resolve, reject) => {
	//setTimeout(() => { resolve('success');}, 2000);
	setTimeout(() => { reject(new Error('fail')); }, 2000);
});

//prom.then((result) => {console.log(result);})
prom.catch((error) => {console.log(error);})
```

- resolve, reject ⇒ 'executor' 함수
- resolve ⇒ fulfilled로 만들수 있는 함수
- reject ⇒ rejected로 만들수 있는 함수

### resolve, reject 사용하여 바로 생성

---

```jsx
const prom1 = new Promise.resolve('success'); // fulfilled 상태의 promise 객체 생성
const prom2 = new Promise.reject(new Error('fail')); // fail 상태의 promise 객체 생성

// prom1은 then의 첫번째 파라메터 실행
// prom2는 then의 두번째 파라메터나 catch
```

### Promise 객체는 언제 생성?

---

1. 전통적인 형식의 비동기 실행 함수를 사용하는 코드를 Promise 기반의 코드로 변환
    - Promise Chaining 안에서 setTimeout 등 비동기 실행 함수 사용시 리턴값을 Promise Chain에서 사용할수 없음.
    - Promise 객체 생성

        ```jsx
        function wait(text, milliseconds) {
          const p = new Promise((resolve, reject) => {
            setTimeout(() => { resolve(text); }, 2000);
          });
          return p;
        }

        fetch('https://jsonplaceholder.typicode.com/users')
          .then((response) => response.text())
          .then((result) => wait(`${result} plus alpha`, 2000))
          .then((result) => { console.log(result); });
        ```

    - Promisify
        - 비동기 실행 함수를 Promise 객체로 감싸서 Promise 객체를 리턴하는 형식으로 만드는 작업
- 콜백을 여러번 하는 경우(setInterval, EventListener)
    - Promisify 해서는 안된다.

        ⇒ 한번 pending에서 fulfilled/rejected 상태가 되면 그 뒤로는 상태와 결과가 바뀌지 않음.

## 여러 Promise 객체

---

### all 메소드

---

```jsx
Promise
.all([프로미스 객체, ... ])
.then((results) => {
	console.log(results); // array
});
```

- 모든 객체가 fulfill 되기까지 기다림
- 하나라도 reject가 되면 all 메소드는 reject 프로미스 리턴
- 하나의 작업이라도 실패하면 전체 작업이 실패할 때 사용

### race 메소드

---

```jsx
Promise
.race([프로미스 객체, ...])
.then((result) => {
	console.log(result); // 단일 객체
});
```

- 가장 먼저 fulfilled / reject 상태가 되는 Promise 객체와 동일한 상태/결과인 Promise 리턴

### allSettled

---

- fulfilled와 rejected 상태를 묶어서 settled 상태라고 한다.
- 배열 내의 모든 Promise 객체가 fulfilled / rejected 상태가 되기까지 기다림,
- 각 Promise 객체의 최종 상태(status), 작업 성공결과(value)/작업 실패정보(reason) 리턴

### any

---

- Promise 객체중 가장 먼저 fulfilled 상태가 된 Promise 객체의 상태/결과 반영
- 모든 Promise 객체가 rejected 상태가 되면 AggregateError 에러 (rejected)

## axios

---

- Ajax 통신을 할 수 있는 또다른 방법

```jsx
axios
  .get('https://jsonplaceholder.typicode.com/users')
  .then((response) => {
    console.log(response);
  })
  .catch((error) => {
    console.log(error);
  });
```

- axios에서도 promise 객체를 리턴한다.
- axios는 별도의 패키지 다운로드가 필요함.
- axios의 기능 / 장점
    - 모든 리퀘스트, 리스폰스에 대한 공통 설정 및 공통된 전처리 함수 삽입 가능
    - serialization, deserialization을 자동으로 수행
    - 특정 리퀘스트에 대해 얼마나 오랫동안 리스폰스가 오지 않으면 리퀘스트를 취소할지 설정 가능(request timeout)
    - 업로드 시 진행 상태 정보를 얻을 수 있음
    - 리퀘스트 취소 기능 지원

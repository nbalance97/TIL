# JSON

---

- JSON 데이터 Response 받는 코드

```jsx
fetch('https://jsonplaceholder.typicode.com/users')
	.then((response) => response.text())
	.then((result) => { console.log(result) });
```

- JSON(JavaScript Object Notation)
    - 자바 스크립트의 문법과 비슷
    - 사용자 하나의 정보를 객체로 표현(중괄호 이내에 프로퍼티)
    - 여러개의 데이터를 대괄호([])로 표현
    - 프로퍼티의 이름을 반드시 큰따옴표("") 사용해주어야 한다.
    - 값이 문자열인 경우 반드시 큰따옴표("") 사용
    - undefined, NaN, Infinity 등 사용 불가능
    - 주석 추가 불가능

    ```jsx
    {
       "attr1":"문자문자열",
       "attr2":0,
       "attr3":15,
       "attr4":["문자열1", "문자열2"]
    }
    ```

## JSON ↔ 객체 변환

---

- 문자열 → JSON 객체 변환

```jsx
fetch('https://jsonplaceholder.typicode.com/users')
	.then((response) => response.text())
	.then((result) => { 
		const users = JSON.parse(result); 
		console.log(users.length);
		console.log(users);
		users.forEach((element) => {
			console.log(element.name);
		});
	});
```

- parse의 결과 배열이 리턴되었음. 아마 가장자리가 []로 되어있어서 그런듯.
- [ ]로 안되있는 경우 테스트.

```jsx
const test = '{"a":"b"}'
console.log(JSON.parse(test));
```

- 그냥 Object가 나타나는것을 볼 수 있음.

## CRUD

---

- 웹 브라우저가 서버로 보내는 리퀘스트의 종류에 크게 4가지 종류
1. GET : 기존 데이터 조회
2. POST : 새 데이터 추가
3. PUT : 기존 데이터 수정
4. DELETE : 기존 데이터 삭제

## Request 테스트

---

- [https://learn.codeit.kr/api/members](https://learn.codeit.kr/api/members)
- 전체 직원(GET)

    ```jsx
    fetch('https://learn.codeit.kr/api/members')
    	.then((response) => response.text())
    	.then((result) => { 
            console.log(result);
    	});
    ```

- 특정 직원(GET)

    ```jsx
    fetch('https://learn.codeit.kr/api/members/3')
    	.then((response) => response.text())
    	.then((result) => { 
            console.log(result);
    	});
    ```

- 직원 추가(POST)
    - fetch에 새로운 argument(옵션 객체) 추가
        - 옵션 객체에 method와 body 추가
    - JSON.stringify : 객체를 JSON 데이터로 변환

    ```jsx
    const newMember = {
    	name: 'Jerry',
    	email: 'jerry@codeitmall.kr',
    	department: 'engineering',
    }

    fetch('https://learn.codeit.kr/api/members', {
    	method: 'POST',
    	body: JSON.stringify(newMember),
    }).then((response) => response.text())
    	.then((result) => { console.log(result) });
    ```

- 직원 변경(PUT)

    ```jsx
    const member = {
    	name: 'Alice',
    	email: 'alice@codeitmall.kr',
    	department: 'marketing',
    }

    fetch('https://learn.codeit.kr/api/members/2', { // 2번 유저에대한 데이터 수정
    	method: 'PUT',
    	body: JSON.stringify(member),
    }).then((response) => response.text())
    	.then((result) => { console.log(result) });
    ```

- 직원 삭제(DELETE)

    ```jsx
    fetch('https://learn.codeit.kr/api/members/2', { // 2번 유저에대한 데이터 삭제
    	method: 'DELETE',
    }).then((response) => response.text())
    	.then((result) => { console.log(result) });
    ```

# REST API 규칙

---

1. URL은 리소스를 나타내기 위해서만 사용하고, 리소스에 대한 처리는 메소드로 표현
    - URL은 리소스를 나타내는 용도, 처리는 메소드에 get, post, put, delete 등으로 표현
2. 도큐먼트는 단수, 컬렉션은 복수 명사

# JSON 데이터 처리

---

```jsx
fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.json())
  .then((result) => { console.log(response); });
```

- response.text 대신 response.json()으로 처리 및 JSON.parse 코드 삭제

# Response

---

- Head : Response에 대한 부가 정보
    - Status Code : 작업 결과
        - 200 : 요청 내용을 서버가 정상 처리
        - 404 : 해당 URL에 해당하는 데이터를 찾을 수 없음.
- Body : 실제 데이터를 담는 부분

# Status Code

---

- 100 Continue
    - 클라이언트가 서버에게 계속 리퀘스트를 보내도 괜찮은지 물어봤을 때, 계속 리퀘스트를 보내도 괜찮다고 알려주는 상태 코드
- 101 Switching Protocols
    - 클라이언트가 프로토콜을 바꾸자는 리퀘스트를 보냈을때 프로토콜을 전환하겠다고 알려주는 상태 코드
- 200 OK
    - 리퀘스트가 성공적으로 처리되었음
- 201 Created
    - 리퀘스트의 내용대로 리소스가 잘 생성됨
- 202 Accepted
    - 리퀘스트의 내용이 일단은 잘 접수됨. 당장 처리되진 않았지만 언젠간 처리
- 301 Moved Permanently
    - 리소스의 위치가 바뀌었음을 나타냄.
    - Location : 리소스에 접근 가능한 새로운 URL
- 302 Found
    - 리소스의 위치가 일시적으로 바뀌었음을 나타냄.
- 304 Not Modified
    - 이전에 response로 받았던 이미지 같은 리소스 재활용 (캐시)
- 400 Bad Request
    - 리퀘스트에 문제가 있음
- 401 Unauthorized
    - 신원이 확인되지 않은 사용자로부터 온 리퀘스트 처리 불가
- 403 Forbidden
    - 신원은 확인되었지만 리소스에 대한 접근 권한이 없음
- 404 Not Found
    - 해당 URL이 나타내는 리소스를 찾을 수 없음
- 405 Method Not Allowed
    - 해당 리소스에 대해 요구한 처리 허용되지 않음
- 413 Payload Too Large
    - request의 body에 들어있는 데이터의 용량이 지나치게 커서 서버가 거부
- 429 Too Many Requests
    - 일정 시간동안 클라이언트가 지나치게 많은 리퀘스트를 보냄
- 500 Internal Server Error
    - 현재 알 수 없는 서버 내의 에러로 인해 리퀘스트를 처리할 수 없다는 뜻
- 503 Service Unavailable
    - 현재 서버 점검 중이거나, 트래픽 폭주 등으로 인해 서비스를 제공할 수 없다는 뜻

```jsx
fetch('https://www.naver.com')
	.then((response) => {
		console.log(response.status); // 상태 코드 출력	
	});
```

# Content-Type 헤더

---

- 데이터들의 타입 정보
1. 주 타입이 text인 경우(텍스트)
    - 일반 텍스트 : text/plain
    - CSS 코드 : text/css
    - HTML 코드 : text/html
    - JavaScript 코드 : text/javascript

1. 주 타입이 image인 경우(이미지)
    - image/bmp : bmp 이미지
    - image/gif : gif 이미지
    - image/png : png 이미지

1. 주 타입이 audio인 경우(오디오)
    - audio/mp4 : mp4 오디오
    - audio/ogg : ogg 오디오

1. 주 타입이 video인 경우(비디오)
    - video/mp4 : mp4 비디오
    - video/H264 : H264 비디오

2. 주 타입이 application인 경우
    - application/json : JSON 데이터
    - application/octet-stream : 확인되지 않은 바이너리 파일 ...

### fetch에 header 추가

---

```jsx
fetch('https://www.naver.com', {
	method: 'GET',
	header: {
		'Content-Type': 'application/json',
	},
})
	.then((response) => {
		console.log(response.status); // 상태 코드 출력	
	});
```

## XML

---

- Content-Type : application/xml
- 시작 태그<attr>와 끝 태그</attr>, 그 사이의 값으로 표현

## application/x-www-form-urlencoded

---

- 프로퍼티의 이름과 값을 이름=값 형식으로 나타내고 각각의 프로퍼티를 '&' 기호로 연결하는 방식
- Percent Encoding 사용하여 URL 인코딩
- URLSearchParam 사용하면 자동으로 값에 URL encoding 적용

```jsx
const urlencoded = new URLSearchParams();
urlencoded.append('attr1', attr1);

fetch('사이트', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
  },
  body: urlencoded,
})
  .then((response) => response.text())
  .then((result) => {
    console.log(result);
  });
```

## multipart/form-data

---

- 여러 데이터를 하나로 묶어서 리퀘스트의 바디에 담아보내려고 할 때 사용되는 중요한 타입
- FormData 객체를 사용해서 전송 가능

```jsx
const formData = new FormData();
formData.append('attr1', attr1);
formData.append('attr2', image.files[0], "attr2.png");

fetch('사이트', {
  method: 'POST',
  body: formData,
})
  .then((response) => response.text())
  .then((result) => { console.log(result); });
```

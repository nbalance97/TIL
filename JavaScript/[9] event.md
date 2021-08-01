## 이벤트 핸들러

---

1. HTML에서 onclick으로 등록 ⇒ 잘 사용하지 않음
2. onclick 등록

```jsx
btn.onclick = function() {
	하는것
};
```

- onclick은 여러번 쓰면 이전것이 없어지는 문제가 있음.

1. addEventListener / removeEventListener
- 가장 권장하는 방법
- 하나의 요소에 여러개의 이벤트 등록할 수 있음.
- removeEventListener → addEventListener에서 등록한 대로 그대로 써주어야 함.
- removeEventListener은 addEventListener으로 등록한 이벤트만 삭제 가능
- 두번째 인자는 함수명!! 함수()로 써주면 리턴값이 들어가게 됨.

```jsx
btn.addEventListener(이벤트, 핸들러);

function event() {
	console.log('hello world');
}
function event2() {
	console.log('world hello');
}

btn.addEventListener('click', event); // event 등록
btn.addEventListener('click', event2); // event 등록 (이전것과 현재것 같이 실행)
btn.removeEventListener('click', event); // event 삭제, 
// 삭제 시 두번째 인자 등록했을 때의 핸들러 그대로

```

### 이벤트

---

1. 마우스 이벤트
    1. mousedown → 마우스 버튼 누를때
    2. mouseup → 마우스 버튼 눌렀다가 뗄때
    3. click → 왼쪽 버튼 클릭
    4. dblclick → 왼쪽 버튼 더블클릭
    5. contextmenu → 오른쪽 버튼
    6. mousemove → 마우스 움직이는 순간
    7. mouseover → 마우스 포인터가 올라온 순간
    8. mouseout → 마우스 포인터가 요소에서 벗어난 순간
    9. mouseenter → 마우스 포인터가 요소 위로 올라온 순간(버블링 x)
    10. mouseleave → 마우스 포인터가 요소에서 벗어난 순간(버블링 x)
2. 키보드 이벤트
    1. keydown → 키보드 누르는 순간
    2. keypress → 키보드 버튼 누르는 순간 (출력되는 키에서만 동작, insert, shift같은건 동작 X)
    3. keyup → 키보드의 버튼을 눌렀다 떼는 순간
3. 포커스 이벤트
    1. focusin : 요소에 포커스 되는 순간
    2. focusout → 요소로부터 포커스가 빠져나가는 순간
    3. focus : 요소에 포커스가 되는 순간(버블링 x)
    4. blur : 요소로부터 포커스가 빠져나가는 순간(버블링 x)
4. 입력 이벤트
    1. change → 입력된 값이 바뀌는 순간
    2. input → 값 입력되는 순간
    3. select → 입력 양식의 하나가 선택되는 순간
    4. submit → 폼 전송하는 순간
5. 스크롤 이벤트
    1. scroll → 스크롤 바 움직일 때
6. 윈도우 창
    1. resize → 윈도우 사이즈 움직일 때

## 이벤트 객체

---

- 이벤트 발생 시 이벤트 객체가 만들어진 후, 이벤트 핸들러의 첫번째 파라메터에 전달

    ```jsx
    function print(event) {
      console.log(event);
    }

    element.addEventListener('keydown', print);
    ```

### 이벤트 객체 프로퍼티

- 공통 프로퍼티
    1. type → 이벤트 이름
    2. target → 이벤트가 발생한 요소
    3. currentTarget → 이벤트 핸들러가 등록된 요소
    4. timeStamp → 이벤트 발생 시간
    5. bubbles → 버블링 단계인지 판단
- 마우스
    1. button → 누른 마우스 버튼 (0 : 왼쪽, 1 : 휠, 2 : 오른쪽)
    2. clientX, clientY → 브라우저 표시 영역에서의 마우스 위치
    3. pageX, pageY → 문서 영역에서의 마우스 위치
    4. offsetX, offsetY → 이벤트 발생한 요소에서의 마우스 위치
    5. screenX, screenY → 모니터 화면 영역에서의 마우스 위치
    6. altKey → alt를 눌렀는지
    7. ctrlKey → ctrl을 눌렀는지
    8. shiftKey → shift키를 눌렀는지
    9. metaKey → meta키를 눌렀는지 (window키 or cmd키)
- 키보드
    1. key → 누른 키가 가지고있는 값
    2. code → 누른 키의 물리적 위치
    3. altKey → alt키 눌렀는지
    4. ctrlKey → ctrl키 눌렀는지
    5. shiftKey → shift키를 눌렀는지
    6. metaKey → meta키를 눌렀는지 (window키 or cmd키)

## 이벤트 버블링

---

- 같은 이벤트에 대해서 부모 요소의 핸들러도 같이 발생하는 현상(document, window까지)
- target 프로퍼티 : 처음 발생한 이벤트로 고정
- currenttarget : 이벤트 핸들러가 동작하는 요소 출력
- e.stopPropagation() → 버블링 멈출 수 있음, 권장하지는 않음.

## 캡처링

---

- 캡처링 : 같은 이벤트가 자식 요소로 전파되는 단계
- 버블링 : 이벤트가 상위 요소로 전파되는 단계
- 타깃 : 이벤트가 실제 타깃 요소에 전달되는 단계
- 캡처링을 사용하기 위해서는 addEventListener의 세번째 프로퍼티
    - true 또는 { capture : true }

### 이벤트 위임

---

- 버블링을 활용
    - 부모 요소에 이벤트 등록 핸들러를 등록 ⇒ 새로운 요소가 들어오더라도 이벤트 등록 가능

    ```jsx
    // 이벤트 위임 (Event Delegation)

    const list = document.querySelector('#list');
    // 부모에 EventListener 등록 
    list.addEventListener('click', function(e) {
    	// 자식 요소의 클릭 -> 부모 요소의 이벤트 핸들러 호출
    	// 클릭한 자식 요소에 대해 이벤트 등록/제거/토글
    	if (e.target.classList.contains('item')) {
    		e.target.classList.toggle('done');
    	}
    });
    ```

    - 단 .. 클릭하기 전까지는 이벤트 등록 안되있으니 두번은 클릭해주어야 함,,

## 브라우저의 기본 동작

---

- 브라우저의 기본 동작이란?
    - 웹페이지에서 우클릭하면 메뉴가 나온다던지 , ....
- event.preventDefault → 브라우저의 기본 동작 제한
    - HTML의 고유역할/의미 훼손할수도 있으니 잘 생각해보고 써야 함.
- 우클릭 제한 예시

    ```jsx
    document.addEventListener('contextmenu', function(e) {
    	e.preventDefault();
    	alert('마우스 오른쪽 클릭은 사용할 수 없습니다.');
    });
    ```

## 마우스 버튼 이벤트

---

- MouseEvent.button
    - 0: 왼쪽, 1: 휠, 2: 오른쪽 버튼
- MouseEvent.type
    - click: 왼쪽
    - contextmenu : 오른쪽 버튼
    - dblclick : 동일 위치에서 빠르게 두번
        - click 이벤트도 두번 발생
    - mousedown : 버튼 누른 순간
    - mouseup : 버튼 눌렀다 뗀 순간
- mousedown→click→mouseup 같이 이벤트 발생 순서를 잘 확인하여야 함.

### 마우스 이동

- mousemove → 마우스 포인터 이동
    - clientX, clientY → 창(브라우저)기준 마우스 포인터 위치
    - pageX, pageY → 웹문서 전체 기준 마우스 포인터 위치
    - offsetX, offsetY →  요소 기준 마우스 포인터 위치
- mouseover → 마우스 포인터가 요소 밖에서 안으로 들어갈때
- mouseout → 마우스 포인터가 요소 안에서 밖으로 나갈때
    - target → 이벤트가 발생한 요소
    - relatedTarget → 이벤트가 발생하기 직전(over)/직후(out) 마우스가 가리키는 요소
- mouseenter / mouseleave = mouseover / mouseout
    - 버블링 일어나지 않음
    - 자식 요소의 영역 계산하지 않음

## 키보드 이벤트

---

### KeyboardEvent.type

- keydown : 키보드 버튼을 누른 순간 ★ 권장
- keypress : 키보드 버튼을 누른 순간 ★ 권장 X
- keyup : 키보드 버튼을 눌렀다 뗀 순간
- key → 이벤트가 발생한 버튼의 값
- code → 버튼의 키보드에서 물리적 위치

## Input 태그

---

- text, password, button, checkbox 등등
- 이벤트
    - focusin : 요소에 포커스가 되었을 때 (클릭되었을 때)
    - focusout : 요소에 포커스가 빠져나갈 때 (다른거 클릭되었을 때)
    - focus → 버블링이 되지 않는 focusin
    - blur → 버블링이 되지 않는 focusout
    - input → 사용자가 입력할 때
    - change → 요소의 값이 변할 때(값이 변하고 focus 이동시 or enter)

- 속성값을 querySelector로
    - document.querySelector('[속성명=속성값]');

## Scroll 이벤트

---

- 웹문서가 스크롤될때 발생(주로 window 객체에 이벤트 핸들러 등록)
- windows.scrollY 값을 기준값 기준으로 비교하면서 이벤트 처리 가능

## Interactive JavaScript

---

## ID로 태그 선택하기

---

- ID는 유일한 값이므로 하나의 요소
- document.getElementById 사용

```jsx
const tag = document.getElementById('id'); 
// 존재하지 않는 id 선택 시 null
```

## Class 속성 사용해서 태그 선택

---

- 여러개의 요소 선택 가능
- document.getElementsByClassName('class')
    - 유사배열 리턴됨.(HTMLCollection, 여러개의 Element이므로)
    - 요소가 없는 경우 빈 HTMLCollection 리턴
    - 순서는 html 태그상 위에서부터 나타남

```jsx
const tags = document.getElementsByClassName('color-btn'); // 유사배열
tags
tags[0]
tags.length 
for (let tag in tags) {
	tag
}

```

- 유사배열(HTMLCollection, NodeList, ...)
    - 숫자 형태 index, length 프로퍼티 존재
    - 배열의 메소드(slice, ..) 사용 불가능
    - Array.isArray(유사배열) → false
    - for .. of 사용 가능

## 태그 사용해서 선택

---

- document.getElementsByTagName('태그')
- 위와 마찬가지로 HTMLCollection 리턴
- 태그에 '*' 주면 모든 태그 선택

```jsx
const tags = document.getElementsByTagName('button') // 모든 버튼 태그
```

## CSS 선택자로 태그 선택

---

- document.querySelector, querySelectorAll
    - 매개변수로는 css 선택자
- document.querySelector('#myNumber') === getElementById('myNumber')
    - id의 경우 맨앞에 #
- document.querySelector('.color-btn') ⇒ class가 color-btn인 요소중 맨 위 하나
    - class의 경우 맨앞에 .
- 맨앞에 #이나 .이 없으면 그냥 태그로 인지하는 듯.
- document.querySelectorAll('.color-btn') ⇒ NodeList로 class가 color-btn인 모든 요소 반환

    == document.getElementsByClassName('color-btn')

- 자식 태그 선택시에는 ..

    document.querySelector('#list li'); // id가 list인 요소의 자식 중 li태그

## 이벤트

---

- 버튼에 onclick 이벤트 작성

    ```jsx
    const btn = document.querySelector('#myBtn');

    // 이벤트 핸들링
    btn.onclick = function () { // 이벤트 핸들러
    	console.log('hello world!');
    }

    // html의 onclick 상에 넣어주는 방법도 있음.
    ```

## Window 객체

---

- window : 전역 객체 (Global Object)
- 웹 브라우저를 대변함.

## DOM(Document Object Model)

---

- 문서 객체 모델
- DOM을 활용하면 HTML 태그를 객체처럼 처리 가능
- document 객체 → 최상단
- document 직접 접근 → HTML 출력
- console.log(document) → DOM 객체를 HTML 형태로 출력
- console.dir(document) → DOM 객체를 객체 형태로 출력

    ```jsx
    const title = document.querySelector('#title');
    title.style.color = 'red';

    ```

- console.log → 값 자체, console.dir→객체의 속성

### DOM 트리

---
![image](https://user-images.githubusercontent.com/76891875/126895530-0824b74e-5373-4693-b1a3-95673406822a.png)

- 같은 레벨 : 형제 노드
- 하위 레벨 : 자식 노드
- 상위 레벨 : 부모 노드
- 요소 노드 → 태그를 표현하는 노드
- 텍스트 노드 → 문자를 표시, 요소 노드의 자식 노드, 자식 노드 가질수 없음.

### 요소 노드 이동 프로퍼티

---

1. 자식 노드

    → tag.children : 해당 요소의 자식노드

    → tag.firstElementChild : 첫번째 자식노드

    → tag.lastElementChild : 마지막 자식노드

2. 부모 노드

    → tag.parentElement

3. 형제 노드

    → tag.previousElementSibling : 이전 형제

    → tag.nextElementSibling : 다음 형제

    → 없으면 null 출력

4. 연쇄 사용

    예시)

    → tag.parentElement.nextElementSibling

### 모든 노드 이동 프로퍼티

---

- Element를 빼면 됨. 이 때는 텍스트 노드도 같이 나타난다.
- 주석 또한 주석 노드로 나타남.
1. node.childNodes
2. node.firstChild
3. node.lastChild
4. node.parentNode
5. node.previousSibling
6. node.nextSibling

### 요소 노드 프로퍼티

---

```jsx
const myTag = document.querySelector('#list-1');

// inner/outer HTML
console.log(myTag.innerHTML); // 자기 포함하지 않은 HTML
myTag.innerHTML += '<h2>hello world!</h2>'; // 이런식으로 태그 추가 가능
console.log(myTag.outerHTML); // 자기 포함해서 전체 HTML, 들여쓰기 포함
outerHTML = '<a id="list-1"><h2>hello world!</h2></a>' 
// 값 대입 시 자기 자신도 전부 사라져버려서 다시 넣어주어야 함.
console.log(myTag.textContent);
// 텍스트 값만 가져옴.
myTag.textContent = 'a'; // 텍스트 수정 가능
myTag.textContent = '<li>hello world!</li>'; // html 태그 그대로 텍스트가 됨. 태그적용 x

```

# 요소 노드

---

- 요소 노드 만들기 → document.createElement('tag')
- 요소 노드 꾸미기 → textContent, innerHTML, ...
- 요소 노드 추가하기 → prepend, append, after, before
- 요소 노드 삭제 → remove

    ```jsx
    today.children[2].remove();
    ```

- 요소 이동

    ```jsx
    // 기존 요소가 없어지고 이동함
    today.append(tomorrow.children[1]); 
    tomorrow.children[1].after(today.children[1]);
    ```

- HTML의 표준 속성 → 요소 노드의 프로퍼티
    - 요소 노드['속성']
    - class속성은 className
- HTML의 표준이 아닌 속성
    - getAttribute(속성명) ⇒ 표준, 표준이 아닌 속성 모두 전달
    - class의 경우 'class'
    - setAttribute(속성명, 값) ⇒ 속성 추가 또는 존재하는 경우 변경
    - removeAttribute(속성명) ⇒ 속성 제거

## 스타일 다루기

---

- 요소.style.textDecoration='line-through'; // 글에 밑줄, 카멜 표기법으로 표시
- 요소.style.backgroundColor = '#DDDDDD';
- style 직접 접근보다는 태그의 class 변경하는 것이 권장
- 요소.className = '스타일이 입혀진 클래스'; ⇒ 통째로 바꿀때
- 요소.classList → 클래스의 속성값을 유사배열로,  ⇒ 클래스의 일부 바꿀때
    - add : 클래스 추가, 요소.classList.add('클래스1', ...);
        - 똑같은 클래스 추가시 알아서 제거됨.
    - remove : 클래스 제거, 요소.classList.remove(클래스명)
    - toggle : 있으면 제거, 없으면 추가, 요소.classList.toggle('', true/false);
        - 두번째 인자가 없어도 되지만 있다면
        - 두번째 인자 true → add만
        - 두번째 인자 false → remove만
        - class 한개만을 다룸.

## 비표준 태그

---

- querySelector로 접근하면 되지만..
    - 후에 필드 생길 문제가 있음
- dataset 프로퍼티 사용
    - data-프로퍼티명

    ```jsx
    document.querySelectorAll('[data-field]')

    // data-프로퍼티명 == 태그.dataset.프로퍼티명
    for (let tag of fields) {
    	const field = tag.dataset.field;
    	tag.textContent = task[field]
    }
    ```


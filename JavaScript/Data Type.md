## 데이터 타입

---

### 기본형

- Number
- String
- Boolean
- Symbol → 유일한 값
- BigInt → 아주 큰 숫자를 다룰 때
- Null
- Undefined

### 참조형

- object

## Symbol, BigInt

---

- Symbol
    - 유일한 값을 가진 변수 이름

    ```jsx
    const symbol = Symbol('aa')
    ```

    - 다른 어떤 값과 비교해도 true가 될 수 없는 고유한 변수
    - 똑같은 설명 붙이더라도 False

- BigInt
    - 기본적으로 JS는 2**53-1 ~ 2**53+1, 해당 범위보다 큰 정수 표현할 때 사용됨.
    - 정수형 뒤에 알파벳 n 붙이거나 BigInt 함수 사용

    ```jsx
    90512512521512n
    BigInt(90512512521512)
    ```

    - 소수 표현 사용 불가능 (1.5n같은 경우 사용 x)
    - 소수 리턴되는 연산은 소숫점 아래 버림

    ```jsx
    10n / 6n // 1n
    ```

    - BigInt끼리만 연산 가능, 서로 다른 타입의 경우 명시적 타입 변환 해주어야 함.

    ```jsx
    5n * 6n
    5n * 6 // 에러
    5n * BigInt(6)
    Number(5n) * 6
    ```

### typeof

---

- typeof(a), typeof a 두가지로 사용 가능
- typeof null → object
- typeof function → function

### Boolean

---

- Falsy 값 : false, null, undefined, NaN, 0, '' ⇒ False 처리
- Truthy 값 : Falsy값이 아닌 나머지 값 ⇒ True 처리

### AND/OR

---

- AND
    - 왼쪽값이 True이면 오른쪽값 리턴
    - 왼쪽값이 False이면 왼쪽값 리턴
- OR
    - 왼쪽값이 True이면 왼쪽값 리턴
    - 왼쪽값이 False면 오른쪽값 리턴

### NULL 병합 연산자(??)

---

- const example = null ?? 'a';
    - 왼쪽에 null이나 undefined → 오른쪽 값 리턴
    - 왼쪽이 null이나 undefined가 아님 → 왼쪽 값 리턴
    - FALSY값 확인하는게 아닌것에 유의 !!

## 변수와 스코프

---

- var : 비권장
    - 변수를 만들기도 전에 사용 가능한 문제(hoisting)
    - 중복 선언 가능한 문제
    - 함수가 아닌 조건문, 반복문 내에서 선언한 변수도 사용가능한 문제
- let, const : 권장
    - 변수 만들기 전에는 접근 불가능
    - 중복 선언 불가능
    - 코드 블록 범위

### 전반적인 세션 개념

- HTTP에서는 새로운 페이지를 요청할 때마다 새로운 연결하여 상태유지가 되지 않음.
- 사용자의 상태를 유지하기 위해 쿠키와 세션 사용
    - 브라우저마다 상태 저장
- 세션의 데이터(사용자의 데이터)는 서버에 저장된다.
- 서버에서는 세션에 데이터를 저장하고, 세션ID를 쿠키로 클라이언트에 발급한다. (Set-cookie)
- 클라이언트는 발급받는 세션ID를 쿠키에 저장한다.
- [https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Sessions](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Sessions)

### 세션의 장점

- 서버에 사용자의 데이터를 저장하므로 보안상 우수

### 세션의 단점

- 서버에 사용자의 데이터를 저장하므로 서버의 부하 증가

### Django에서의 Session

---

- 세션에 데이터를 저장하기 전까지는 Sessionid를 따로 클라이언트에 전송하지 않음.
- 세션에 데이터를 저장하는 순간부터 Sessionid를 클라이언트에 전달
- Django에서의 세션 사용법

```python
def f(request):
	...
	request.session['저장할 데이터명'] = 값
```

- request.session : 현재 사용자/브라우저에 대한 세션, 딕셔너리처럼 사용 가능
- 세션에 데이터를 간접적으로 저장한 경우 따로 Django에 modified를 설정하여 데이터를 저장했음을 알려주어야 함.
    - 예를들면 session에 리스트가 저장되어 있고, 리스트의 요소중 하나의 값을 바꿧다던지..

```python
request.session.modified = True
```

### Session 관련 Setting 몇가지(Settings.py)

---

- 자세한 정보는 [https://docs.djangoproject.com/en/3.2/ref/settings/#sessions](https://docs.djangoproject.com/en/3.2/ref/settings/#sessions)
1. SESSION_COOKIE_AGE
    - 세션 쿠키 지속기간(초), 기본값은 2주
2. SESSION_COOKIE_HTTPONLY
    - 세션 쿠키에 HttpOnly 설정, 기본값은 True
    - 세션 쿠키를 JavaScript에서 읽는건 문제가 될 수 있으므로 False를 굳이??
3. SESSION_COOKIE_NAME
    - 세션 쿠키의 이름, 기본값은 sessionid
4. SESSION_COOKIE_PATH
    - 세션 쿠키의 경로, 기본값은 /
5. SESSION_COOKIE_SECURE
    - SECURE 옵션 사용여부, https를 사용한다면 True로 하는게 좋음.
    - 기본값은 False
6. SESSION_SAVE_EVERY_REQUEST
    - 모든 request의 session 데이터 저장
    - 원래는 session 데이터가 변경된 경우에만 저장
    - session이 텅 빈 경우는 True라 하더라도 생성되지 않음.

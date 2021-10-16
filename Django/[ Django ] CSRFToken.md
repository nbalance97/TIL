- CSRF 공격을 막기 위한 방법
- CSRF 토큰이란 다른 사이트에서 접근할 수 없는 특정 사이트에 고유한 비밀값이라고 보면 된다.
- Django에서는 CsrfViewMiddleware에 의해 설정된다.
- 원래는 form태그에서 {% csrf_token %} 태그를 설정

```python
{% csrf_token %}
```

- CsrfViewMiddleware에서는 먼저, POST에 있는 csrfmiddlewaretoken을 찾고, 없다면 X-csrftoken을 찾는다.
- X-CSRFToken의 경우는 Ajax 등 api에서 사용된다.

### 과정

---

1. response에 set_cookie를 설정하여 csrf_token 쿠키를 넣어서 전달한다.
2. request에서는 전달받은 csrf_token을 POST의 csrfmiddlewaretoken에 넣어서 전달하거나, 아니면 헤더에 추가한다. ⇒ X-CSRFToken에 넣어서 전달
3. 서버에서는 쿠키에 존재하는 csrf_token의 값하고 csrfmiddlewaretoken이나 X-CSRFToken의 값을 비교하는 방식으로 진행한다.

### 관련 데코레이터

---

- ensure_csrf_cookie 데코레이터 추가
    - csrf_token set-cookie 추가

```python
@method_decorator(ensure_csrf_cookie)
def post(self, request, *args, **kwargs):
```

- csrf_protect 데코레이터 추가
    - csrf_token 인증 진행

```python
@method_decorator(csrf_protect)
def post(self, request, format=None):
```

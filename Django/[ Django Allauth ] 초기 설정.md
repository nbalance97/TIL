### Django allauth 설치 방법

---

- [공식 문서](https://django-allauth.readthedocs.io/en/latest/installation.html)를 참고
1. pip install django-allauth
2. Installed_apps에 추가

    ```python
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    ```

3. 다음 구문 추가

    ```python
    SITE_ID = 1

    AUTHENTICATION_BACKENDS = [
        # Needed to login by username in Django admin, regardless of `allauth`
        'django.contrib.auth.backends.ModelBackend',

        # `allauth` specific authentication methods, such as login by e-mail
        'allauth.account.auth_backends.AuthenticationBackend',
    ]
    ```

4. Email 세팅

    ```python
    EMAIL_BACKEND = "django.core.mail.backends.console.EmailBackend"
    # 콘솔로 이메일 보내도록 설정
    ```

5. url 설정 ( 프로젝트 앱)

    ```python
    path('accounts/', include('allauth.urls')),
    # account/login, account/signup 등 자동완성

    path('', include('allauth.urls')) # /login, /signup 등
    ```

### User Setting

---

- Settings.url 맨 아래에

    ```python
    AUTH_USER_MODEL = '앱.모델'
    ```

    - allauth가 만들어둔 모델을 사용하도록 하기 위함
- migration시 모델 생성 / 세팅까지 모두 한 후에 진행
    - 충돌 문제가 있을수 있음.

### 로그아웃 시 확인메세지 없이 바로 로그아웃

---

```python
ACCOUNT_LOGOUT_ON_GET = True
```

### Redirect 처리

---

- 로그인 / 회원가입 시 이동 URL 처리
    - 기본값 : accounts/profile로 리다이렉팅

```python
ACCOUNT_SIGNUP_REDIRECT_URL = "index"
LOGIN_REDIRECT_URL = "index"
```

### django.contrib.sites

---

- 어떤 기능을 여러 웹사이트에서 사용 가능하도록
- 장고 프로젝트 하나로 여러 웹사이트 운영 가능
- SITE_ID : 각각의 사이트에 대한 ID
    - Settings.py에 설정

        ```python
        SITE_ID = 1
        ```

### 실습 중간에 만난 에러

---

1. DoesNotExist at /admin/ Site matching query does not exist.

    ⇒ SITE_ID = 1 안써줬다고 나는 에러임..
    

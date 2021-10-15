- 이전에는 DjangoRestFramework-jwt를 사용하였으나, 찾아보니 DjangoRestFramework-jwt가 더이상 업데이트 되지 않으며, simplejwt 사용이 권장되고 있다고 함.

### 설치

---

```python
# 구버전 ㅂㅇ..
pip uninstall djangorestframework-jwt

# 신버전 ㅎㅇ~ 
pip install djangorestframework-simplejwt
```

### Settings.py 설정

---

- INSTALLED_APPS에 추가

```python
INSTALLED_APPS = [
    ...
    'rest_framework_simplejwt',
    ...
]
```

- 인증방식에 JWTAuthentication 추가

```python
REST_FRAMEWORK = {
    ...
    'DEFAULT_AUTHENTICATION_CLASSES': (
        ...
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
    ...
}

```

### urls.py 설정

---

- TokenObtainPairView
    - POST 방식의 API
    - username, password를 넘겨주면 만약 사용자 인증이 된다면 토큰 발급
    - 기존 djangorestframework_jwt와 다르게 refresh, access 토큰이 따로 발급된다.
- TokenRefreshView
    - POST 방식의 API
    - refresh 토큰을 넘겨주면 새로운 토큰을 발급받음.(access 토큰)

- 여기서, 실제로 우리가 사용하게 될 토큰은 access 토큰이라고 보면 된다.

```python
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)

urlpatterns = [
    ...
		path('apitokenauth/', TokenObtainPairView.as_view()),
    path('apitokenrefresh/', TokenRefreshView.as_view()),
    ...
]
```

### 쿠키 설정

---

- 사용자 브라우저로 하여금 쿠키를 설정하게 하기 위해선 Set-cookie 헤더를 넘겨주어야 한다.
- response의 set_cookie 메서드를 사용하여 쿠키를 설정해 준다.
- 예제에서는 쿠키의 키를 'JWT'로 설정하였으며 httponly를 설정하여 Script에서는 읽지 못하도록 하였음.

```python
class TokenObtainPairView_ud(TokenObtainPairView):
    def post(self, request, *args, **kwargs):
        response = super().post(request, *args, **kwargs)
        if response.status_code != 200:
            return response

        response.set_cookie(
            key = 'JWT',
            value = response.data['access'],
            max_age = 300,
            httponly = True,
            samesite = 'Lax'
        )
        return response

# urls.py는 아래로 당연히 바꾸어주어야 함.
path('apitokenauth/', TokenObtainPairView_ud.as_view()),
```

### request → 쿠키

---

- 서버에서 set_cookie 헤더가 포함된 response를 전달하면, 클라이언트에서는 키값이 'JWT'인 쿠키가 생성되어 있을 것이다.
- 클라이언트에서 서버로 다음에 쿠키가 포함된 request를 보낼 때, 서버에서는 쿠키를 읽어서 인증하는 과정이 필요하다.
- 따라서, 쿠키에서 JWT 토큰을 가져와서 읽고, 토큰을 검증하면 된다.
    - JWTAuthentication을 커스터마이징

```python
class Custom_JWTAuthentication(JWTAuthentication):
    def authenticate(self, request):
        raw_token = request.COOKIES.get('JWT', None)
        if raw_token is None:
            return None
        
        validated_token = self.get_validated_token(raw_token)
        return self.get_user(validated_token), validated_token

class APITest(APIView):
    authentication_classes = [Custom_JWTAuthentication]

    def get(self, request, format=None):
        return Response({'message': 'success call api'})
```

- set-cookie를 직접 사용해 보면서 쿠키에 대해 좀 더 알게된 것 같지만, cors 문제가 생기는 경우 브라우저에 쿠키가 설정되지 않는 문제점이 발견되었다.
    - CORS 관련 허용과 CREDENTIALS 설정을 해주어도 안되서 템플릿 자체를 서버에 넣어서 해결하긴 했으나.. 근본적인 해결책은 아닌것 같아서 조금 더 해봐야 할 것 같다.

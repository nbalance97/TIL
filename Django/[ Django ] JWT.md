[https://jwt.io/introduction](https://jwt.io/introduction)

- 로그인 시 Token 발급
- 발급된 Token으로 서비스 이용 가능
- JWT Web Token의 구조는 다음과 같다.
    - Header.Payload.Signature

---

### Header

- Header에는 Token의 type과 알고리즘(SHA256이나 RSA)으로 구성
    - Base64Url 방식으로 인코딩되어 있다.

### Payload

- 개체(유저 등)의 상태나 부가 정보가 포함된 여러개의 claim으로 구성
- registered, public, privated claim이 존재한다.
    
    ### Registered claims
    
    - (iss)issuer, (exp)expiration time, (sub)subject, (aud)audience 등 유용한 정보 제공
    - claim의 이름은 3글자
    
    ### Public claims
    
    - 마음대로 정의 가능하나 충돌 방지하기 위해서는 IANA JSON 웹 토큰 레지스트리에 정의하거나 충돌 방지를 포함하는 URI 정의
    
    ### Private claims
    
    - 정보 공유를 위해 생성된 사용자 지정 claim

### Signature

- 인코딩된 Header, 인코딩된 payload, 키(secret), 알고리즘을 가져와서 서명한 부분
- 메세지가 도중에 변경되지 않았는지 확인하는데 사용

---

- JWT Token : Header, Payload, Signature을 모두 합침.

## Django에서 JWT 사용

---

- Django의 라이브러리 중 JWT를 편하게 사용할 수 있는 라이브러리인 DjangoRestFramework-jwt 사용
- [https://jpadilla.github.io/django-rest-framework-jwt/](https://jpadilla.github.io/django-rest-framework-jwt/)
1. **DjangoRestFramework-jwt 설치**
    
    ```python
    pip install djangorestframework-jwt
    ```
    

1. **Settings.py에 다음 내용 추가**
    
    ```python
    REST_FRAMEWORK = {
        'DEFAULT_PERMISSION_CLASSES': (
            'rest_framework.permissions.IsAuthenticated',
        ),
        'DEFAULT_AUTHENTICATION_CLASSES': (
            'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
            'rest_framework.authentication.SessionAuthentication',
            'rest_framework.authentication.BasicAuthentication',
        ),
    }
    ```
    

1. **urls.py에 다음 내용 추가**
    - url명은 자유로이 변경 가능
    - obtain_jwt_token ⇒ jwt token 획득
        - POST 메서드, data로 username, password 주면 token 획득
    - refresh_jwt_token ⇒ jwt token 갱신
        - POST 메서드, data로 기존 token 주면 새로운 토큰을 response
    - verify_jwt_token ⇒ jwt token 검증
        - POST 메서드, data로 token 주어야 함.
    
    ```python
    path('apitokenauth/', obtain_jwt_token),
    path('apitokenrefresh/', refresh_jwt_token),
    path('apitokenverify/', verify_jwt_token),
    ```
    

1. Token 생성 / 사용 테스트
    - 간단한 api를 만들어 주는데, authentication을 JSONWebTokenAuthentication으로 해주어야 JWT를 사용하여 인증이 진행된다.
    
    ```python
    path('api/', APITest.as_view())
    
    class APITest(APIView):
        authentication = [JSONWebTokenAuthentication]
    
        def get(self, request, format=None):
            return Response({'message': 'success call api'})
    ```
    
    - JavaScript로 테스트
        - cors 문제가 발생하여 API 호출이 잘 안되는 문제점이 있었음.
        - django-cors-headers 라이브러리 추가
            
            ```python
            pip install django-cors-headers
            ```
            
        - 현재는 API 접근 제한을 따로 걸 필요가 없으므로 전체 허용(CORS_ALLOW_ALL_ORIGINS)
        
        ```python
        # INSTALLED_APPS에 추가
        INSTALLED_APPS = [
            ...,
            "corsheaders",
            ...,
        ]
        
        # Settings.py의 맨 아래에 추가
        CORS_ALLOW_ALL_ORIGINS=True
        
        # MIDDLEWARE에 추가, 적어도 CommonMiddleware보다는 위에 가도록
        MIDDLEWARE = [
            ...,
            "corsheaders.middleware.CorsMiddleware",
            "django.middleware.common.CommonMiddleware",
            ...,
        ]
        ```
        
        - 자세한 점은 [https://github.com/adamchainz/django-cors-headers](https://github.com/adamchainz/django-cors-headers)
        
        ```jsx
        let userdata = {
        	username : 'admin',
        	password : 1234,
        };
        
        async function get_JWT_token() {
        	try {
        		const response = await fetch('http://127.0.0.1:8000/apitokenauth/', {
        			method: "POST",
        			headers: {
        				"Content-Type": "application/json"
        			},
        			body: JSON.stringify(userdata)
        		});
        		const data = await response.json();
        		const token = data["token"];
        		console.log(token);
        		return token;
        	} catch (error) {
        		console.log('error');
        	}
        };
        
        async function call_api() {
        	let api_token = await get_JWT_token();
        	const token = api_token;
        	const called_api = await fetch('http://127.0.0.1:8000/test/api/', {
        			method: "GET",
        			headers: {
        				"Content-Type": "application/json",
        				"Authorization": "JWT " + token
        			}
        		});
        	const api_result = await called_api.json();
        	return api_result;
        };
        ```
        
        - 오랜만에 async / await 써보니까 많이 헷갈림..
        - 소스를 전체적으로 설명하자면 username이 'admin', password가 1234인 유저의 JWT Token을 얻은 다음, 토큰을 가지고 API를 호출하는 소스이다.
        - API 호출 시에는 headers에 Authorization을 추가해주어야 한다. 값은  JWT + " " + 토큰
        - 유저 데이터가 서버에 없는 경우 JWT 토큰이 발급되지 않으며 당연히 API 호출 시에도 unauthorized가 나타난다.
        - 크롬 개발자 도구에서 console.log가 잘 출력되지 않는다면 필터에 뭐가 입력되어 있는지, 아니면 정보 부분이 체크가 잘 되어있는지 확인해야 한다.
        - 토큰을 로컬 저장소에 저장하거나, 쿠키에 저장할 수 있다는데.. JS를 통해 로컬 저장소에 있는 토큰을 가져올 수 있어서 XSS 공격에 취약하고, 쿠키같은 경우는 secure, httpOnly 속성을 활용하여 막을 수 있다고 한다. 쿠키에 저장하는 방법이 그나마 나아보이긴 하지만, CSRF 문제가 있을 수 있다고 한다.
            - [https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)

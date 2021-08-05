## Django API 만들기

### 1. 추가 설치

```bash
pip install djangorestframework
```

### 2. serializer 생성

- queryset, 모델 → json, xml 쉽게 변환
- [serializers.py](http://serializers.py) 생성

```python
from rest_framework import serializers
from .models import InterestCompany

class InterestCompanySerializer(serializers.ModelSerializer):
    class Meta:
        model = InterestCompany
        fields = "__all__" # Tuple 형식으로 나열해주어도 된다.
```

### 3. ViewSet 생성

- CRUD를 1개의 View로 구현
    - [GET] api/{pk}/ : detail
    - [PUT] api/{pk}/ : 수정
    - [DELETE] api/{pk}/ : 삭제
    - [GET] api/ : 전체 리스트
    - [POST] api/ : 추가
- permission_classes 등록이나 생성 시 save 처리 필요(perform_create)

```python
from .serializer import InterestCompanySerializer
from rest_framework import viewsets

class InterestCompanyViewSet(viewsets.ModelViewSet):
    queryset = InterestCompany.objects.all()
    serializer_class = InterestCompanySerializer
```

### 4. URL 설정

- 앱의 urls.py에 다음과 같이 작성

```python
IC_list = InterestCompanyViewSet.as_view({
    'post': 'create',
    'get': 'list'
})

IC_detail = InterestCompanyViewSet.as_view({
    'get': 'retrieve',
    'put': 'update',
    'patch': 'partial_update',
    'delete': 'destroy'
})

path('interest/api/', IC_list, name="api-company-list"),
path('interest/api/<int:pk>/', IC_detail, name="api-company-detail"),
```

⇒ 보았을 때.. 해당 url로 접근하였을 때, post/get/put/patch/delete 등 접근 방식에 따라 취하는 행동 정의하는 거 같음.

### 5. 테스트

- TemplateDoesNotExist(rest_framework/api.html) 에러 등장
    - 구글링 결과 .. INSTALLED_APPS에 등록을 안함.
    
    
![image](https://user-images.githubusercontent.com/76891875/126649305-c613f623-5d99-4185-bb2a-d490cf3f80a7.png)


- [Settings.py](http://settings.py) → INSTALLED_APPS → rest_framework 등록

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework'
    'Interest'
]
```

## Ajax 설정

---

- jquery load

```python
<script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
```

- Ajax Script 작성

```jsx
<!--버튼-->
<a class="btn btn-success" id="success" onclick="clickevent(this);" name="{{forloop.counter0}}">관심 기업 등록</a>
<!-- 자기 자신 전달하기 위해선 this로 넘겨주면 된다. -->

function clickevent(self) {
        var idx = $(self).attr('name')
        var name = $('#name'+idx).attr('name')
        var title = $('#title'+idx).attr('name')
        console.log(idx+"//"+name+"//"+title);
        $.ajax({
            type: "POST",
            url: "{% url 'api-company-list' %}",
            data: {
                "company_name" : name,
                "intern_title" : title,
                "duration" : "1900-01-01T12:00"
            },
            dataType: "json",
            success: function(response) {
                window.location.reload()
            },
            error: function(request, status, error) {
                console.log("code:"+request.status+"\n"+"message:"+request.responseText+"\n"+error);

            }
        });
    }
```

- Django CSRF 에러시 해결법
    - 인증을 아직 도입하지 않았을 때, 필요 없는경우

    ```python
    from rest_framework.authentication import SessionAuthentication, BasicAuthentication

    class CsrfExemptSessionAuthentication(SessionAuthentication):
        def enforce_csrf(self, request):
            return  # To not perform the csrf check previously happening

    #ModelViewSet 맨 윗줄에 추가
    authentication_classes = (CsrfExemptSessionAuthentication, BasicAuthentication)
    ```

    - [https://stackoverflow.com/questions/30871033/django-rest-framework-remove-csrf](https://stackoverflow.com/questions/30871033/django-rest-framework-remove-csrf)

## CSRF Token 실어서 Ajax 사용
## Django Ajax 사용시 인증 처리

---

- csrf_token을 실어서 보내주어야 한다.
    - csrf_token은 로그인 할때마다 바뀐다.
    1. getCookie 함수를 사용하여 csrftoken을 얻어온다.
    2. ajax에서 beforesend 프로퍼티를 통해 X-CSRFToken 헤더를 추가해서 request를 전송

        ⇒ 에러 없이 동작하는것을 볼 수 있다.

reference : [https://docs.djangoproject.com/en/3.2/ref/csrf/](https://docs.djangoproject.com/en/3.2/ref/csrf/)

```jsx
const csrftoken = getCookie('csrftoken');

function getCookie(name) {
    let cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        const cookies = document.cookie.split(';');
        for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}

$.ajax({
            type: "POST",
            url: "{% url 'api-company-list' %}",
            data: {
                "user" : userId,
                "company_name" : name,
                "intern_title" : title,
                "duration" : "1900-01-01T12:00"
            },
            dataType: "json",
            beforeSend: function(request) {
                request.setRequestHeader('X-CSRFToken', csrftoken);
            },
            success: function(response) {
                window.location.reload()
            },
            error: function(request, status, error) {
                console.log("code:"+request.status+"\n"+"message:"+request.responseText+"\n"+error);

            }
        });
```

### User 모델

---

1. User → 기본 유저 모델
    - 추후에 필드 추가할 시 마이그레이션의 어려움 등 어려워서 권장되지 않음.
2. AbstractUser → 상속받아서 커스터마이징 가능한 유저 모델
    - 원하는 필드 추가 가능
3. AbstractBaseUser → 상속받아서 커스터마이징 가능

### AbstractUser

---

- username
- password
- first_name
- last_name
- email
- date_joined
- last_login

```jsx
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

### AbstractBaseUser

---

- email, password, nickname, ..
- 모두 커스텀된 필드 사용할 때 상속받아서 사용하면 된다.

## 유효성 검사
- [Settings.py](http://settings.py) 언어타입 변경

    ```python
    LANGUAGE_CODE = "ko"
    ```

- 닉네임 유효성검사 메세지 변경 ( unique에 걸리게되는 경우 )

    ```python
    nickname = models.CharField(
            max_length=15, 
            unique=True, 
            null=True,
            error_messages={'unique': '이미 사용중인 닉네임입니다.'}
        )
    ```

- validation.py
    - string.punctuation ⇒ 특수문자만 모아놓은 리스트

    ```python
    def contains_special_character(value):
        for char in value:
            if char in string.punctuation:
                return True
        return False

    def validate_no_special_characters(value):
        if contains_special_character(value):
            raise ValidationError("특수문자를 포함할 수 없습니다.")
    ```

    ### 클래스형 validator

    1. 클래스 생성

        ```python
        class CustomPasswordValidator:
            def validate(self, password, user=None):
                if (
                        len(password) < 8 or
                        not contains_uppercase_letter(password) or
                        not contains_lowercase_letter(password) or
                        not contains_number(password) or
                        not contains_special_character(password)
                ):
                    raise ValidationError("8자 이상의 영문 대/소문자, 숫자, 특수문자 조합이어야 합니다.")

            def get_help_text(self):
                return "8자 이상의 영문 대/소문자, 숫자, 특수문자 조합을 입력해 주세요."
        ```

    2. settings.py에 등록

        ```python
        AUTH_PASSWORD_VALIDATORS = [
            {
                "NAME": "coplate.validators.CustomPasswordValidator",
            }
        ]

        ACCOUNT_PASSWORD_INPUT_RENDER_VALUE = True # 유효성 오류나도 password 사라지지 않도록
        ```

### Model에 validator 등록

---

```python
nickname = models.CharField(
        max_length=15, 
        unique=True, 
        null=True,
        validators=[validate_no_special_characters],
        error_messages={'unique': '이미 사용중인 닉네임입니다.'}
    )
```

### User 접근

---

**현재 로그인중인 유저 접근**

- View ⇒ request.user
- Template ⇒ {{ user }}
- 유저 객체가 리턴됨. [user.email](http://user.email) 등으로 접근 가능

**로그인되지 않은 유저 처리**

- AnonymousUser가 리턴 됨.
- request.user.is_authenticated : 유저가 로그인 되었는지 안되었는지 확인

### User 동작원리

---

- 로그인 → 유저 세션 생성
- 서버는 세션을 저장 + 브라우저에게 sessionid 전송
- 브라우저는 sessionid를 쿠키에 저장 / 요청시 쿠키(sessionid) 실어서 전달
- 서버는 이를 통해 로그인이 되었는지 안되었는지 확인
- Logout 버튼 눌러서 직접 로그아웃 ⇒ 서버의 세션 삭제
- 쿠키 유효시간 만료 ⇒ 서버의 세션 유지
    - 세션 데이터가 쌓이는 문제가 있으므로 세션을 주기적으로 초기화 해주어야 함.

    ```python
    python manage.py clearsessions
    ```

- 웹브라우저 종료하더라도 로그인 유지(Settings.py)

    ```python
    ACCOUNT_SESSION_REMEMBER = True
    ```

- 쿠키의 유효시간 설정(Settings.py)

    ```python
    SESSION_COOKIE_AGE = 3600 # 1시간
    ```

### login_required 데코레이터

---

- 만약 유저가 로그인되지 않았다면?

    ⇒ settings.LOGIN_URL로 리다이렉팅 한다.

- 유저가 로그인 상태라면?

    ⇒ 정상적으로 실행

- 함수형 view에서 주로 쓰인다.

```jsx
@login_required(login_url='common-login')
def InterestCompanyAllDelete(request):
    object = InterestCompany.objects.filter(user=request.user)
    object.delete()
    return redirect('company-list')
```

### LoginRequiredMixin

---

- 클래스 기반의 뷰를 사용할때 LoginRequiredMixin을 쓰면 login_required와 같음
- 로그인되지 않은 사용자의 요청 ⇒ 로그인 페이지로 리다이렉션 or 403 에러 (raise_exception 파라메터)
- 제네릭 뷰에서 들어오는 request 확인

    ⇒ self.request

```jsx
class InterestCompanyCreateView(**LoginRequiredMixin**, CreateView):
    model = InterestCompany
    form_class = InterestCompanyForm
    template_name = 'Interest/interestcompany_create.html'
    **login_url = 'common-login'**

    def get_success_url(self):
        return reverse('company-list')

    def form_valid(self, form):
        form.instance.user = self.request.user # user 객체 추가
        return super().form_valid(form)
```

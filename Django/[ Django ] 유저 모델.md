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

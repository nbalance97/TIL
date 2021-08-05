### 모델 변경

---

```python
class User(AbstractUser):
    nickname = models.CharField(max_length=15, unique=True)
    def __str__(self):
        return self.email
```

### Migration 두가지 방법

---

1. unique=True 삭제 → Default 닉네임 설정 → Admin 페이지에서 닉네임 수정
2. null = True 추가 → Admin 페이지에서 닉네임 수정
    - allAuth에서는 모델을 기본적으로 생성하고 null로 비어있는걸 채움

    ```python
    nickname = models.CharField(max_length=15, unique=True, null=True)
    ```

- 관리자 사이트에 추가한 필드 넣어줌.
    - admin.py

        ```python
        UserAdmin.fieldsets += (("Custom fields", {"fields": ("nickname",)}),)
        ```

### 회원가입 폼 커스터마이징

---

- forms.py

    ```python
    class SignupForm(forms.ModelForm):
        class Meta:
            model = User
            fields = ["nickname"]
        
        def signup(self, request, user):
            user.nickname = self.cleaned_data["nickname"]
            user.save()
    ```

- settings.py

    ```python
    ACCOUNT_SIGNUP_FORM_CLASS = "coplate.forms.SignupForm"
    ```

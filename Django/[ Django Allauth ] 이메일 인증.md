### Settings.py

- mandatory, optional, none
- 순서대로 인증하지 않으면 로그인 불가, 이메일 인증하지 않아도 로그인 가능, 이메일 인증 발송하지 않음.

```python
ACCOUNT_EMAIL_VARIFICATION = "mandatory" # default : optional
```

- 로그인 하면 터미널에 이메일이 와있는걸 알수있음.
    - 링크 클릭시 인증되도록 setting

    ```python
    ACCOUNT_CONFIRM_EMAIL_ON_GET = True
    ```

### urls.py

- 인증 완료시 인증 완료 되었다는 메세지 표시하는 페이지로 리다이렉션

```python
from django.views.generic import TemplateView

path("email-confirmation-done/"
	,TemplateView.as_view(template_name="coplate/email_confirmation_done.html"
	,name="account_email_confirmation_done"),
```

### Settings.py

```python
# 로그인 되었을 때
ACCOUNT_EMAIL_CONFIRMATION_AUTHENTICATED_REDIRECT_URL = 'account_email_confirmation_done'

# 로그인 되지 않았을 때
ACCOUNT_EMAIL_CONFIRMATION_ANONYMOUS_REDIRECT_URL = 'account_email_confirmation_done'
```

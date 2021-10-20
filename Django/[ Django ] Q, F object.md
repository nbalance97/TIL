### Import

---

```python
from django.db.models import Q, F
```

### Q object

---

- Django DB 관련 작업에서 사용할 수 있는 SQL 조건문
- filter 메서드에서 주로 사용
- OR이나 AND 연산자나 NOT 연산자를 사용하여 조건을 정의하기 위해 사용된다.
    
    ```python
    Question.objects.filter(Q(subject__icontains=query) or Q(content__icontains=query))
    ```
    
    - 각 조건을 Q object로 만들고, 두개의 Q object를 각각 or 연산
    - or( | ), and( & ), not( ~ )
    

### F Object

- 쿼리 내에서 모델 필드 접근할 때 사용
- 모델 필드나 annotated column의 값을 가져올 때 사용된다.

```python
from django.db.models import F
Entry.objects.filter(number_of_comments__gt=F('number_of_pingbacks'))
```

- 표현식과 함께 사용할 때는 반드시 유의해야 한다.
    - F 함수를 만나게 되면 캡슐화된 SQL문을 생성하기 위해 파이썬 연산자를 오버라이딩 한다.

```python
# Tintin filed a news story!
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed += 1
reporter.save()

###############################################

from django.db.models import F

reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed = F('stories_filed') + 1
reporter.save()

# reporter.refresh_from_db(fields=['stories_filed'])

reporter.name = 'Tintin Jr.'
reporter.save() # 
```

- 위 소스를 아래 소스로 바꾸려고 하는건데.. 문제가 있다.
- 아래 소스는 F('stories_filed') + 1 자체를 SQL Syntax로 만들어버려서 reporter.stories_filed의 값이 1 증가하는 쿼리가 실행된다. 이 때, reporter.stories_filed의 값을 갱신하는것이 아님. 데이터베이스 자체의 SQL문 실행
- reporter.stories_filed 값을 실제 저장된 값으로 업데이트하기 위해선 모델을 데이터베이스에서 다시 불러와주어야 한다.
- F Object는 save() 이후에도 모델에 남아있는 문제가 있으며, 각 save()에 적용된다. 따라서 초기 값이 1이라고 할 때, 처음 save 연산자를 해주면 SQL문이 실행되어 2가 되는데, 다음에 또 save 연산을 해주면 또 SQL문이 실행되어 3이 된다.이 과정을 해결해주기 위해서는 refresh_from_db()를 사용해주면 된다.
    - Model.refresh_from_db(using=None, fields=None)

### pk

- get을 할때, pk로 입력하나 id로 입력하나 같은 결과가 나온다.
- pk가 id__exact와 같다고 한다.

```python
model.objects.get(id=1)
model.objects.get(pk=1)
```

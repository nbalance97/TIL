### Pagination

---

1. Paginator을 import 해준다.

    ```python
    from django.core.paginator import Paginator
    ```

2. 소스 작성
    1. 현재 페이지번호를 가져온다.
    2. Paginator(리스트, 한번에 보여줄 갯수)로 Paginator을 만들어 준다.
    3. Paginator의 get_page(페이지번호)를 사용하여 해당 페이지번호의 요소들을 가져와서 context로 넘겨준다.

```python
def get_page_number(self):
        page_number = self.request.GET.get('page')
        if page_number is None:
            return 1
        else:
            return page_number

def get_context_data(self, **kwargs):
        context = super(InterestCompanyListView, self).get_context_data(**kwargs)
        internship_info = get_internship_information()
        page = self.get_page_number()
        total_page_list = Paginator(internship_info, 9)
        curr_page = total_page_list.get_page(page)
        context['internship_information'] = internship_info
        context['internship_obj'] = curr_page
        return context
```

3. 템플릿에서의 사용
- get_page의 리턴값.object_list 사용
``` html
{% for object in internship_obj.object_list %}
    <div class="col-md-4">
        <div class="card text-white bg-primary mb-3" style="max-width: 20rem; margin:10px;">
          <div class="card-header" style="font-family:BMJUA;">{{object.company_name}}</div>
          <div class="card-body">
              <input type="hidden" id="name{{forloop.counter0}}" name="{{object.company_name}}">
              <input type="hidden" id="title{{forloop.counter0}}" name="{{object.title}}">
              <h4 class="card-title" style="font-family:BMJUA;">{{object.title}}</h4>
              <p class="card-text" style="font-family:BMJUA;">{{object.duration}}</p>
              <a class="btn btn-success" id="success" onclick="clickevent(this);" name="{{forloop.counter0}}">관심 기업 등록</a>
          </div>
        </div>
    </div>
{% endfor %}
```

4. page_number 처리
- has_next, has_previous : 이전/이후 페이지 존재여부 검사
- previous_page_number(이전페이지), number(현재 페이지), paginator.num_pages(마지막 페이지)
- next_page_number(다음 페이지)
```html
<div class="row justify-content-md-center" style="margin:auto">
{% if internship_obj.has_previous %}
    <a href="?page=1" class="col-sm-1 btn btn-warning"><<</a>
    <a href="?page={{ internship_obj.previous_page_number }}" class="col-sm-1 btn btn-warning"><</a>
{% else %}
    <a class="col-sm-1"></a>
    <a class="col-sm-1"></a>
{% endif %}
    <div class="col-sm-3" style="margin: 10px 0px; text-align: center; font-family:BMJUA;">
        Page {{ internship_obj.number }} of {{ internship_obj.paginator.num_pages }}.
    </div>
{% if internship_obj.has_next %}
    <a href="?page={{ internship_obj.next_page_number }}" class="col-sm-1 btn btn-warning">></a>
    <a href="?page={{ internship_obj.paginator.num_pages }}" class="col-sm-1 btn btn-warning">>></a>
{% else %}
    <a class="col-sm-1"></a>
    <a class="col-sm-1"></a>
{% endif %}

</div>
```

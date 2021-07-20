# Django 제네릭 뷰

## 1. BaseViews

- TemplateView
- RedirectView

## 2. Display Views

- ListView
- DetailView

## 3. Edit View

- FormView
- CreateView
- UpdateView
- DeleteView

## 4. DateViews

- YearArchiveView
- MonthArchiveView
- DayArchiveView
- TodayArchiveView

## 사용 시 [urls.py](http://urls.py) 수정

- import 필요(예시)

```python
from django.views.generic import (
    CreateView, ListView, DetailView, UpdateView, DeleteView, RedirectView
)
```

- View.as_view()로 사용해 주어야 함.

```python
from django.contrib import admin
from django.urls import path, include
from posts import views

urlpatterns = [
    # path('', views.index, name='index'),
    path('', views.IndexRedirectView.as_view(), name='post-list'),
    #path('posts/', views.post_list, name='post-list'),
    path('posts/', views.PostListView.as_view(), name='post-list'),
    #path('posts/new', views.post_create, name='post-create'),
    path('posts/new', views.PostCreateView.as_view(), name='post-create'),
    # path('posts/<int:post_id>/', views.post_detail, name='post-detail'),
    path('posts/<int:post_id>/', views.PostDetailView.as_view(), name='post-detail'),
    #path('posts/<int:post_id>/edit', views.post_update, name='post-update'),
    path('posts/<int:post_id>/edit', views.PostUpdateView.as_view(), name='post-update'),
    #path('posts/<int:post_id>/delete', views.post_delete, name='post-delete'),
    path('posts/<int:post_id>/delete', views.PostDeleteView.as_view(), name='post-delete'),
]
```

### RedirectView

- 새로운 URL로 이동시키는 기능
- 속성
    - url : 이동할 url을 지정하는 속성, 기본값 : None
    - pattern_name : URL 패턴의 이름을 지정하는 속성, 기본값 : None
    - query_string : 쿼리 스트링을 전달할 지 여부, 기본값 : None

```python
class IndexRedirectView(RedirectView):
    pattern_name = 'post-list'
```

### DetailView

- 상세 페이지를 볼 때 주로 사용
- 속성
    - model : 사용할 모델 지정, 기본값 : None
    - pk_url_kwarg : 단일 데이터 조회 시 사용할 키워드 인수, 기본값 : "pk"
    - template_name : 렌더할 템플릿 지정, 기본값 : "None"
    - context_object_name : 템플릿으로 전달할 모델의 context 키워드, 기본값 : '모델명'

```python
class PostDetailView(DetailView):
    model = Post
    template_name = 'posts/post_detail.html'
    pk_url_kwarg = 'post_id'
    context_object_name = 'post'
```

### ListView

- 목록 페이지에서 주로 사용
- 속성
    - model : 사용할 모델 지정, 기본값 : None
    - ordering : 데이터 정렬 속성 지정, 기본값 : None
    - paginate_by : 페이징 시 표시할 데이터의 수, 기본값 : None
    - page_kwarg : 쿼리스트링으로부터 가져올 현재 페이지에 대한 키워드, 기본값 : 'page'
    - template_name : 렌더할 템플릿 지정 속성, 기본값 : 'None'
    - context_object_name : 템플릿으로 전달할 모델의 context 키워드 지정, 기본값 : '모델명_list'

```python
class PostListView(ListView):
    model = Post
    template_name = 'posts/post_list.html'
    context_object_name = 'posts'
    ordering = ['-dt_created'] # 내림차순
    paginate_by = 6
    page_kwarg = 'page' # 현재 페이지
```

### CreateView

- 새로운 데이터를 생성하기 위한 뷰
- 속성
    - model : 사용할 모델 지정, 기본값 : None
    - form_class : 입력을 위한 Form Class 지정, 기본값 : None
    - fields : 데이터 입력에 사용할 필드 명시적으로 지정, 기본값 : None
    - template_name : 렌더할 템플릿을 지정하는 속성, 기본값 : None
    - pk_url_kwarg : 보여주기 위한 단일 데이터를 조회할 때 사용할 키워드 인수 지정, 기본값 :'pk'
    - success_url : 처리가 성공했을 때 리디렉션 할 URL을 지정

```python
class PostCreateView(CreateView):
    model = Post
    form_class = PostForm
    template_name = 'posts/post_form.html'
    # 자동으로 form이라는 키워드로 PostForm 전달

    def get_success_url(self):
        return reverse('post-detail', kwargs={'post_id': self.object.id})
```

### UpdateView

- 기존 데이터 개체의 수정을 위한 뷰
- 속성
    - model : 사용할 모델 지정, 기본값 : None
    - form_class : 입력을 위한 Form class 지정, 기본값 : None
    - field : 데이터 입력에 사용할 필드 명시 지정, 기본값 : None
    - template_name : 렌더할 템플릿을 지정하는 속성, 기본값 : None
    - pk_url_kwarg : 보여주기 위한 단일 데이터 조회 시 사용할 키워드 인수, 기본값 : 'pk'
    - success_url : 처리가 성공했을 때 리다이렉션 할 url, 기본값 : None
    - context_object_name : 템플릿으로 전달할 모델의 context 키워드, 기본값 : '모델명'

```python
class PostUpdateView(UpdateView):
    model = Post
    form_class = PostForm
    template_name = 'posts/post_form.html'
    pk_url_kwarg = 'post_id' # url에서 받는 기본키 값

    def get_success_url(self):
        return reverse('post-detail', kwargs={'post_id':self.object.id})
```

### DeleteView

- 기존 데이터 삭제하는 기능을 위한 뷰
- 속성
    - model : 사용할 모델 지정, 기본값 : None
    - template_name : 렌더할 템플릿을 지정하는 속성, 기본값 : None
    - pk_url_kwarg : 보여주기 위한 단일 데이터 조회 시 사용할 키워드 인수 지정, 기본값 : 'pk'
    - success_url : 처리가 성공했을 때 리디렉션 할 URL 지정, 기본값 : None
    - context_object_name : 템플릿으로 전달할 모델의 context 키워드 지정, 기본값 : '모델명'

```python
class PostDeleteView(DeleteView):
    model = Post
    template_name = 'posts/post_confirm_delete.html'
    pk_url_kwarg = 'post_id'
    context_object_name = 'post'

    def get_success_url(self):
        return reverse('post-list')
```

## Context 전달

### Default

- 모델 데이터 → 'object' or '모델명(소문자)'
- 여러개의 데이터 → 'object_list' or '모델명(소문자)_list'
- Post의 경우 object, post 두개로 사용 가능
- 여러개의 데이터 → 'object_list' or 'post_list'

### 변형

- context_object_name을 변경해주면 된다.
- 여러개의 데이터일때, context_object_name = 'posts'

    ⇒ posts, object_list 두개로 사용 가능

### 각 View별 몇몇개 Context

1. CreateView
    1. form : form_class에 명시한 폼 전달
2. ListView
    1. paginator : Paginator 객체 전달
    2. page_obj : 현재 Page 객체 전달
    3. is_paginated : 페이지네이션 적용 여부
3. UpdateView
    1. form : form_class에 명시한 폼 전달

# 제네릭 뷰에 Context로 추가 인자 넘겨주어야 하는 경우

- 전달해야하는 Context 변수가 더 있는 경우

```python
class InterestCompanyListView(ListView):
    model = InterestCompany
    template_name = 'Interest/interestcompany_list.html'
    context_object_name = 'objects'

    def get_context_data(self, **kwargs):
        context = super(InterestCompanyListView, self).get_context_data(**kwargs)
        context['internship_infor'] = get_internship_information()
        return context
```

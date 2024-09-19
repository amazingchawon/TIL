# Django URLs

1. URL dispatcher
    - dispatcher : 운항 관리자, 분배기
    - URL 패턴을 정의하고 해당 패턴이 일치하는 요청을 처리할 view 함수를 연결(매핑)

## Variable Routing

1. 현재 URL 관리의 문제점
    - 템플릿의 많은 부분이 중복되고, URL의 일부만 변경되는 상황이라면 계속해서 비슷한 URL과 템플릿을 작성해나가야 할까?
2. 정의 : URL 일부에 변수를 포함시키는 것, 변수는 view 함수의 인자로 전달 가능
3. 작성법:
    - `<path_converter:variable_name>`
    - path converter : URL 변수의 타입을 지정 (str, int 등 5가지 타입 지원)

## App과 URL

1. App URL mapping
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/5025afe3-ff24-484f-a82f-3c1335a6b1cd/82d7de0c-3e5f-4587-9065-3fdef3a7c746/image.png)
    
    - 각 앱에 URL을 정의하는 것
    - 프로젝트와 각 앱이 URL을 나누어 관리를 편하게 하기 위함

```html
# 프로젝트 이름/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path('articles/', include('articles.urls')),
]
```

```html
# articles/urls.py -> 앱 폴더에 있는 urls 파일

from django.urls import path
# 명시적 상대경로
from . import views

urlpatterns = [
    path('index/', views.index),
    path('dinner/', views.dinner),
    path('search/', views.search),
    path('throw/', views.throw),
    path('catch/', views.catch),
    path('hello/<str:name>/', views.greeting),
]
```

1. `include()`
    - 프로젝트 내부 앱들의 URL을 참조할 수 있도록 매핑하는 함수
    - URL의 일치하는 부분까지 잘라내고, 남은 문자열 부분은 후속 처리를 위해 include된 URL로 전달

## URL 이름 지정

1. URL 구조 변경에 따른 문제점
    - 기존 `index/` 주소가 `articles/index/` 로 변경됨에 따라 해당 url을 사용하는 모든 위치를 찾아가 변경해야 함
    - 해결 : URL에 이름을 지어주면, 이름만 기억하면 됨

### Naming URL patterns

1. 설명 :
    
    ```html
    path('index/', views.index, name='index`)
    ```
    
    - URL에 이름을 지정하는 것
    - path 함수의 name 인자를 정의해서 사용
2. URL 표기 변화
    
    ```html
    # 기존
    <a href='dinner/'>dinner<a>
    
    # 변경 후
    <a href='{% url 'dinner' %}'>dinner<a>
    ```
    
    - url을 작성하는 모든 곳에서 변경
        - a 태그의 href 속성 값 뿐만 아니라 form의 action 속성 등도 포함
3. `url` tag : `{% 'url name' arg1 arg2 %}`
    - 주어진 URL 패턴의 이름과 일치하는 절대 경로 주소를 반환

## URL 이름 공간

1. URL 이름 지정 후 남은 문제
    - 서로 다른 앱의 url이름이 같은 경우 발생
    - 이름만으로는 완벽하게 분리할 수 없음 → 이름에 성(key)를 붙이자
2. `app_name` 속성
    
    ```html
    # 앱 이름/urls.py
    
    app_name = 'articles'
    urlpatterns = {
    	...,
    }
    ```
    
    - 각 앱의 [`url.py`](http://url.py) 파일 상단에 app_name 변수 값 설정
3. URL tag의 최종 변화 : `{% url 'app_name:path_name' %}`
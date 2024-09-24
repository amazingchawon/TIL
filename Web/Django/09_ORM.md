# ORM Objective Relational Mapping

1. 정의 : 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 기술
2. 역할 :
    - Django와 DB 간에 사용하는 언어가 다르기 때문에 소통 불가
    - Django에 내장된 ORM이 중간에서 이를 해석
        - django : 파이썬 ← **ORM** →  Database : SQL
        - point! ORM 문법을 어떻게 활용해야 하는가!

### 용어 정리

1. Query :
    - 데이터베이스에 특정한 데이터를 보여 달라는 요청
    - “쿼리문을 작성한다.”
        - “원하는 데이터를 얻기 위해 데이터베이스에 요청을 보낼 코드를 작성한다.”
    - python으로 작성한 코드가 ORM에 의해 SQL로 변환되어 데이터베이스에 전달되며, 데이터베이스의 응답 데이터를 ORM이 QuerySet이라는 자료 형태로 변환하여 우리에게 전달
2. QuerySet
    - 데이터베이스에게서 전달 받은 객체 목록(데이터 모음)
        - 순회가 가능한 데이터로써 1개 이상의 데이터를 불러와 사용할 수 있음
    - Django ORM을 통해 만들어진 자료형
    - 단, 데이터베이스가 단일한 객체를 반환 할 대는 QuerySet이 아닌 모델(Class)의 인스턴스로 반환됨

## QuerySet API

![image.png](https://soshace.com/wp-content/uploads/2023/03/orm-1024.jpg)

1. 설명:
    - ORM에서 데이터를 검색, 필터링, 정렬 및 그룹화 하는 데 사용하는 도구
        - API를 사용하여 SQL이 아닌 Python 코드로 데이터를 처리
    - 파이썬의 모델 클래스와 인스턴스를 활용해 DB에 데이터를 저장(C), 조회(R), 수정(U), 삭제(D)하는 것
    - QuerySet API를 통해 ORM 문법 사용
    - ORM은 QuerySet이나 Instance를 반환
2. 사용시 유용한 외부 라이브러리
    
    ```bash
    pip install ipython django-extensions
    ```
    
    - django_extensions → 프로젝트의 `settings.py` 에 `INSTALLED_APPS` 에 추가해주어야 함
3. Django shell
    
    ```bash
    python manage.py shell_plus
    ```
    
    - django 환경 안에서 실행되는 python shell
        - 쉘에 입력하는 QuerySet API 구문이 django 프로젝트에 영향을 미침
    - model들을 import해주어서 편리

### QuerySet API 구문

1. 구조
    
    ```python
    Article.objects.all()
    ```
    
    - `Article` : Model class
    - `objects` : Manager
    - `all()` : QuerySet API
        - 전체 조회 구문
2. `all()` : 전체 조회

## CRUD

1. 정의 : 소프트웨어가 가지는 기본적인 데이터 처리 기능
    - Create : 저장
    - Read : 조회
    - Update : 갱신
    - Delete : 삭제

### Create

1. 객체 생성
    - 방법 1 : class로부터 instance 생성
        - 인스턴스 변수를 활용하여 정보 저장
    - 방법 2 : class로부터 instance 생성, 인자를 넘겨주어 정보 저장
    - 방법 3 : QuerySet API 중 create() 매서드 활용
2. 저장
    - `인스턴스.save()` : 객체를 데이터베이스에 저장하는 인스턴스 메서드

### Read

1. 메소드
    - Return new QuerySets
        - `all()` : 전체 데이터 조회
            - `Article.objects.all()`
        - `filter()` : 주어진 매개변수와 일치하는 객체를 포함하는 QuerySet 반환
            - `Article.objects.filter(content='django!')` : content가 django!인 쿼리셋 줘!
    - Do not return QuerySets (단일 객체 조회 가능)
        - `get()` : 주어진 매개변수와 일치하는 **객체**를 반환
            - `Article.objects.get(pk=1)` : 1번 게시글 줘!
            - 객체를 찾을 수 없을 때 : `DoesNotExist` 예외 발생
            - 객체가 둘 이상 일 때 : `MultipleObjectsReturned` 예외 발생
            - 이러한 특징으로 인해 primary key와 같이 고유성(uniqueness)을 보장하는 조회에서 사용해야 함

### Update

1. 절차
    - 수정할 인스턴스 조회 → `get()`
    - 인스턴스 변수를 변경
    - 저장 → `save()`

### Delete

1. 절차
    - 수정할 인스턴스 조회 → `get()`
    - 인스턴스 변수를 삭제 → `delete()`
        - 삭제된 객체가 반환
2. 참고
    - 삭제한 데이터는 더 이상 조회 할 수 없음
    - django는 지워진 데이터의 id는 재사용하지 않음

## ORM with view 실습

django shell에서 연습했던 QuerySet API를 직접 view 함수에서 사용하기

### 전체 게시글 조회

```python
# artcles/views.py -> 앱의 views.py 파일에서

from django.shortcuts import render
# 모델 클래스 가져오기
from .models import Article

# Create your views here.
def index(request):
    # 게시글 전체 조회 요청 to DB
    # 모델클래스.objects.all() -> 지금 여기서 모델 클래스 사용하기 위해서는 import 필요
    articles = Article.objects.all()
    context = {
        'articles' : articles,
    }
    return render(request, 'articles/index.html', context)
```

```html
<!--artivles/index.html-->

<body>
    <h1>Articles</h1>
    {% for article in articles%}
        <p>글 번호 : {{ article.pk }}</p>
        <p>글 제목 : {{ article.title }}</p>
        <p>글 내용 : {{ article.content }}</p>
        <hr>
    {% endfor %}
</body>
```

# 참고

## Field lookups

1. Query에서 조건을 구성하는 방법
2. QuerySet 메서드 `filter()`, `exclude()` 및 `get()`에 대한 키워드 인자로 지정됨

```jsx
# Field lookups 예시

# 내용에 'dja'가 포함된 모든 게시글 조회
Ariticle.objects.filter(content__contains='djan')

# 제목이 he로 시작하는 모든 게시글 조화
Article.objects.filter(title__startswith='he)
```

## ORM, QuerySet API를 사용하는 이유

1. 데이터베이스 추상화
    - 개발자는 특정 데이터베이스 시스템에 종속되지 않고 일관된 방식으로 데이터를 다룰 수 있음
2. 생산성 향상
    - 복잡한 SQL 쿼리를 직접 작성하는 대신 Python 코드로 데이터베이스 작업을 수행 할 수 있음
3. 객체 지향적 접근
    - 데이터베이스 테이블을 Python 객체로 다룰 수 있어 객체 지향 프로그래밍의 이점을 활용할 수 있음
# Django 구조

## 가상 환경 생성 루틴 간단 정리

1. Django 프로젝트 생성 전 루틴
    - 가상 환경 생성 : `python -m venv venv`
    - 가상 환경 활성화 : `source venv/Script/activate`
    - Django 설치 : `pip install django`
    - 패키지 목록 파일 생성 (패키지 설치시마다 진행) : `pip freeze > requirements.txt`
2. Django 프로젝트 생성 루틴
    - `.gitignore` 파일 생성 (첫 add 전) : `touch .gitignore`
    - git 저장소 생성 : `git init`
    - Django 프로젝트 생성 : `django-admin startproject config .`

## Django 설치 / 서버

1. Django 설치
    
    ```bash
    pip install django
    ```
    
    - 버전을 명시 하지 않으면, 현재 파이썬 환경에서 설치 할 수 있는 최신 패키지가 설치됨
    - 4.2.16 설치
2. Django 프로젝트 생성
    
    ```bash
    django-admin startproject config .
    ```
    
3. Django 서버 실행
    
    ```bash
    python manage.py runserver
    ```
    
4. 서버 종료
    - ctrl + C

## Project & App

1. Django project
    - 애플리케이션의 집합
    - DB 설정, URL 연결, 전체 앱 설정 등을 처리
2. Django application
    - 독립적으로 작동하는 기능 단위 모듈
    - 각자 특정한 기능을 담당하며 다른 앱들과 함께 하나의 프로젝트를 구성

### Project

1. 프로젝트 생성
    
    ```bash
    django-admin startproject config .
    ```
    
2. 프로젝트 구조
    - `settings.py` : 프로젝트의 모든 설정을 관리
    - `urls.py` : 요청 들어오는 URL에 따라 이에 해당하는 적절한 views를 연결
        - MTV 패턴의 V, APP 폴더에 있음
    - `__init__.py` : 해당 폴더를 패키지로 인식하도록 설정하는 파일
    - `asgi.py` : 비동기식 웹 서버와의 연결 관련 설정
    - [`wsgi.py`](http://wsgi.py) : 웹 서버와의 연결 관련 설정
    - `manage.py` : Django 프로젝트와 다양한 방법으로 상호작용하는 커맨드 라인 유틸리티

### App

1. 앱 생성
    
    ```bash
    python manage.py startapp (앱 이름)
    ```
    
    - 앱 이름은 복수형으로 지정하는 것을 권장
    - 앱 폴더는 프로젝트 폴더 안에 생기는 것이 아님
2. 앱 등록
    - Project 폴더 `settings.py` 안에 `INSTALLED_APPS` 라는 이름의 리스트 → 제일 윗 부분에 앱 추가
        - `INSTALLED_APPS` 에 이미 적혀있는 항목들 : 내장 앱
    - 제일 윗 부분에 추가하는 이유 :
        - `INSTALLED_APPS` 의 구동 순서 : 위 → 아래
        - 내장 앱이 마지막에 실행되는 것이 안정적
    - 등록 후 생성 불가능 → 생성 후 등록하도록 유의
3. 앱 구조
    - `admin.py` : 관리자용 페이지 설정
    - `models.py` : DB와 관련된 Model을 정의(MTV 패턴의 M)
    - `views.py` : HTTP 요청을 처리하고 해당 요청에 대한 응답을 반환(url, model, template과 연계)(MTV 패턴의 V)
    - `apps.py` : 앱의 정보가 작성된 곳
    - `tests.py` : 프로젝트 테스트 코드를 작성하는 곳
    - `__init__.py` : 프로젝트에 있는 `__init__.py`와 똑같은 역할

## 요청 & 응답

![요청 응답](https://velog.velcdn.com/images/guswlsdl0121/post/2e850764-deb6-4308-a143-f4aa6ab588cc/image.png)

1. URLS :
    
    ```python
    # urls.py
    
    # 기본적으로 import 되어 있는 것
    from django.contrib import admin
    from django.urls import path
    # 내가 추가한 이름
    from (앱 이름) import views
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        # path(url 경로, 함수)
        path('index/', views.index),
    ]
    ```
    
    - 웹 사이트를 구현할 때, 페이지마다 url 지정 필요
        - Django에서는 이를 urls.py 파일을 통해 구현
    - url 경로는 반드시 ‘/’(slash)로 끝나야 함
    - 함수 호출 안하는 이유
        - 함수 호출은 안되고, 함수에 대한 주소만 저장하는 것
        - 요청이 왔을 때 호출을 해야해서 주소만 저장
2. View :
    
    ```python
    # views.py
    
    from Django,shortcuts import render
    
    def index(request):
    	return render(request, template_name, context)
    ```
    
    - urls.py 에 들어가는 함수 혹은 클래스를 정의하는 곳
    - 특정 경로에 있는 tamplate과 request 객체를 결합해 응답 객체를 반환
        - 매개변수 이름 : 관습적으로 request를 사용
    - `render` 함수
        - 주어진 템플릿을 주어진 컨텍스트 데이터와 결합하고 렌더링 된 텍스트와 함께 HttpResponse 응답 객체를 반환하는 함수
        - `request` : 응답을 생성하는 데 사용되는 요청 객체
        - `template_name` : 템플릿 이름의 경로
        - `context` : 템플릿에서 사용할 데이터 (딕셔너리 타입으로 작성)
3. Templates :
    - 절차
        - 앱 폴더 안에 templates 폴더 생성
            - 폴더명은 반드시 templates여야 하며 개발자가 직접 생성해야 함
        - template 폴더 안에 템플릿 파일 생성
            - 파일이 또 다른 폴더 안에 있어도 무관
            - Django가 인식하는 기본 경로 : `app 폴더 / templates /`
                - view 함수에서 template 경로 작성 시 이 지점 이후의 경로를 작성해야 함
4. 주의 : 데이터 흐름에 따른 코드 작성
    ![코드 작성 순서](https://blog.kakaocdn.net/dn/bgKncD/btr4tNXSziS/ryLO0dTrpfnu22uEj0QDKk/img.png)
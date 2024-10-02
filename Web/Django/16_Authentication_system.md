# Django Authentication System

1. 정의 : 사용자 인증과 관련된 기능을 모아 놓은 시스템
2. Authentication 인증
    - 권한과 헷갈리지 말기
    - 사용자가 자신이 누구인지 확인하는 것, 신원 확인

## 실습

- urls.py 를 앱안에 만들었을 때→ 처음에는 urlpatterns 리스트가 비어있어서 이걸 아예 안 작성하는 경우가 있음 → 이러면 애러가 남
- custom user model로 대체
- 기존 마이그레이션 파일, 데이터 베이스 지우기
- 수퍼유저 생성

# Custom User model

1. User Model
    - 여태까지 사용 했음
    - admin 이 만들어진다는거 자체가 User를 관리하는 Model이 있다는 뜻
    - 한번도 본 적이 없음
2. 기본 User Model의 한계
    - 우리는 지금까지 별도의 User 클래스 정의 없이 내장된 auth 앱에 작성된 User 클래스를 사용했음
        - `settings.py` 에서 `INSTALLED_APPS`에 `'django.contrib.auth'`라는 앱이 내부적으로 User 클래스를 가지고 있음
            - 이 user 클래스는 `AbstractUser`을 상속 받음
    - Django의 기본 User 모델은 username, password 등 제공되는 필드가 매우 제한적
    - 추가적인 사용자 정보(생년월일, 주소, 나이 등)가 필요하다면 이를 위해 기본 User Model을 변경하기 어려움
        - 별도의 설정 없이 사용할 수 있어 간편하지만, 개발자가 직접 수정하기 어려움
3. User Model 대체의 필요성
    - 프로젝트의 특정 요구사항에 맞춰 사용자 모델을 확장할 수 있음

## Custom User Model로 대체하기

1. AbstractUser 클래스를 상속받는 custom User 클래스 작성
    
    ```python
    from django.contrib.auth.models import AbstractUser
    from django.db import models
    
    # Create your models here.
    # 커스텀 User 모델로 대체하기 위한 class 작성
    class User(AbstractUser):
        pass
    ```
    
    - 기존 User 클래스도 Abstact User를 상속 받음 → 이 클래스만 상속받으면 custom User 클래스도 기존 User 클래스와 완전히 같은 모습을 가지게 됨
2. `settings.py` 변경 :
    
    ```python
    # settings.py
    
    AUTH_USER_MODEL = 'accounts.User'
    ```
    
    - Django 프로젝트에서 사용하는 기본 User 모델을 우리가 작성한 User 모델로 사용할 수 있도록 `AUTH_USER_MODEL` 값을 변경
    - `AUTH_USER_MODEL` : django 프로젝트의 User를 나타내는 데 사용하는 모델을 지정하는 속성
        - 프로젝트 중간에 `AUTH_USER_MODEL`을 **변경 할 수 없음**
            - 이미 프로젝트가 진행되고 있을 경우, 데이터베이스 초기화 후 진행
    - 기본값 : `auth.User` (앱.class 이름)
    - 변경값 : `accounts.User`
3. admin site에 대체한 User 모델 등록
    
    ```python
    # accounts/admin.py
    
    from django.contrib import admin
    from django.contrib.auth.admin import UserAdmin
    from .models import User
    
    # Register your models here.
    admin.site.register(User, UserAdmin)
    ```
    
    - 기본 User 모델이 아니기 때문에 등록하지 않으면 admin 페이지에 출력이 되지 않기 때문

## 프로젝트를 시작하며 반드시 User 모델을 대체해야 함

1. 이유 :
    - Django는 새 프로젝트를 시작하는 경우 비록 기본 User 모델이 충분하더라도 커스텀 User 모델을 설정하는 것을 강력하게 권장하고 있음
    - 커스텀 User 모델은 **기본 User 모델과 동일하게 작동**하면서도 **필요한 경우 나중에 맞춤 설정** 할 수 있기 때문
2. 주의 :
    - User Model 대체 작업은 프로젝트의 모든 migrations 혹은 첫 migrate을 실행하기 전에 작업을 마쳐야 함

### 초기화 방법

1. migrations 파일 지우기
2. 데이터 베이스 지우기

# Login

1. 정의 : 로그인은 Session을 Create 하는 과정
2. 필요한 함수(로직) : 2개 (요청 주소가 같고 요청 메소드만 다른 것이어서 1개로 합쳐지기는 함)
    - GET : 로그인 페이지 응답
    - POST : 인증/로그인 진행

## AuthenticationForm()

1. 설명 : 로그인 인증에 사용할 데이터를 입력 받는 built-in form
    - ModelForm이 아닌, Form
        - 이유 : 로그인 할 때 입력하는 아이디, 비밀번호 → 저장 X, 단지 인증 수단일 뿐
        - DB로 저장하는 것이 아니기 때문에 Form을 사용
        - 반면, 회원가입에 사용하는 Form은 ModelForm

## 실습

1. `urls.py`
    
    ```python
    from django.urls import path
    from . import views
    
    app_name = 'accounts'
    urlpatterns = [
        path('login/', views.login, name='login'),
    ]
    ```
    
2. `view`
    
    ```python
    from django.shortcuts import render, redirect
    from django.contrib.auth.forms import AuthenticationForm
    from django.contrib.auth import login as auth_login
    
    # Create your views here.
    def login(request):
        if request.method == 'POST':
            # ModelForm과 인자 구성이 다름
            form = AuthenticationForm(request, request.POST)
            if form.is_valid():
                # 만약 인증된 사용자라면 로그인 진행(세션 데이터 생성)
                # Form에는 save()가 없음
                # auth_login(요청 객체, 인증된 유저 객체)
                # 인증된 유저 객체 -> AuthenticationForm 객체인 form에 정보 저장되어 있음
                # AuthenticationForm 제공 메소드인 get_user() 사용
                auth_login(request, form.get_user())
                # 다른 앱의 페이지로 이동 가능
                return redirect('articles:index')
        else:
            form = AuthenticationForm()
        context = {
            'form': form,
        }
        return render(request, 'accounts/login.html', context)
    ```
    
    - `login(request, user)` : AuthenticationForm을 통해 인증된 사용자를 로그인 하는 함수
        - 로그인 == 세션 데이터 생성
    - `get_user()` : AuthenticationForm의 인스턴스 메서드
        - 유효성 검사를 통과했을 경우, 로그인 한 사용자 객체를 반환
3. Login 성공했는지 알아보는 법
    - 방법 1 : 개발자 도구를 사용해서 cookie에 session id가 있으면 성공
    - 방법 2 : django_session 테이블에서 확인

# Logout

1. 설명 : Session을 Delete 하는 과정

## 실습

1. `logout(request)`
    - DB에서 현재 요청에 대한 Session Data를 삭제
    - 클라이언트의 쿠키에서도 Session Id를 삭제
2. urls
    
    ```python
    # accounts/urls.py
    
    urlpatterns = [
    	...
    	path('logout/', views.logout, name='logout')
    ]
    ```
    
3. view
    
    ```python
    # accounts/views.py
    from django.contrib.auth import logout as auth_logout
    
    def logout(request):
    	auth_logout(request)
    	return redirect('articles:index')
    ```
    
4. Template
    
    ```html
    <form action="{% url "accounts:logout" %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="LOGOUT">
    </form>
    ```
    
5. login은 a태그이고, logout은 form인 이유
    - login : get 요청, 페이지를 달라고함
    - logout : 데이터 베이스 조작 → post 사용 필요

# Template with Authentication data

1. 템플릿과 인증 데이터
    - 템플릿에서 로그인 여부 확인하기 어려움
    - 템플릿에서 인증 관련 데이터를 출력하는 방법
2. 방법
    
    ```html
    <!-- index.html -->
    
    {% comment %} user 객체 넘겨준적 X {% endcomment %}
    <p>안녕하세요 {{ user.username }}</p>
    ```
    
3. `user.user.name`이 가능한 이유
    - django가 미리 준비한 context 데이터가 존재하기 때문 → context processors

## context processors

1. 설명 :
    - 템플릿이 렌더링 될 때 호출 가능한 컨텍스트 데이터 목록
    - 작성된 컨텍스트 데이터는 기본적으로 템플릿에서 사용 가능한 변수로 포함됨
    - → django에서 자주 사용하는 데이터 목록을 미리 템플릿에 로드 해 둔 것
2. 코드
    
    ```python
    # settings.py
    
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    # 이거
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ] 
    ```
# Login

1. 정의 : 로그인은 Session을 Create 하는 과정
2. 필요한 함수(로직) : 2개 (요청 주소가 같고 요청 메소드만 다른 것이어서 1개로 합쳐지기는 함)
    - GET : 로그인 페이지 응답
    - POST : 인증/로그인 진행

## `AuthenticationForm()`

1. 설명 : 로그인 인증에 사용할 데이터를 입력 받는 **built-in form**
    - ModelForm이 아닌, Form
        - 이유 : 로그인 할 때 입력하는 아이디, 비밀번호 → 저장 X, 단지 인증 수단일 뿐
        - DB로 저장하는 것이 아니기 때문에 Form을 사용
        - 반면, 회원가입에 사용하는 Form은 ModelForm

## 실습

1. `urls.py`
    
    ```python
    # accouns/urls.py

    from django.urls import path
    from . import views
    
    app_name = 'accounts'
    urlpatterns = [
        path('login/', views.login, name='login'),
    ]
    ```
    
2. `view`
    
    ```python
    # accounts/views.py

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
3. Template
   ```HTML
   <!-- accounts/login-html --->
   <h1>로그인</h1>
   <form action="{% url 'accounts:login' %}" method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        <input type="submit">
    </form>
    ```
4. Login 성공했는지 알아보는 법
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
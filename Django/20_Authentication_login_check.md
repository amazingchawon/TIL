# 인증된 사용자에 대한 접근

1. 로그인 사용자인지 아닌지 확인하는 2가지 방법
    - `is_authenticated` 속성
    - `login_required` 데코레이터

## `is_authenticated`

1. 설명 : 사용자가 인증 되었는지 여부를 알 수 있는 User model의 속성
    - 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성
    - 비인증 사용자에 대해서는 항상 False
2. 적용
    - html 파일에서 if 문을 사용해 로그인/비로그인 상태에 따라 출력되는 링크를 다르게 설정
    
    ```html
    <!-- articles/index.html -->
    
    {% if request.user.is_authenticated %}
    	<h3>Hello, {{ user.username }}</h3>
    		<a href="{% url 'articles:create' %}">NEW</a>
    		<form action="{% url 'accounts:logout' %}" method="POST">
    			{% crsf_token %}
    			<input type="submit" value="Logout">
    		</form>
    		더 적어야 함
    {% else %}
    	<a href="{% url 'articles:login' %}">Login</a>
    	<a href="{% url 'articles:signup' %}">Signup</a>
    {% endif %}
    ```
    
    - 인증된 사용자라면 로그인/회원가입 로직을 수행할 수 없도록 하기
    
    ```python
    # accounts/views.py
    
    def login(request):
    	if request.user.is_authenticated:
    		return redirect('article:index')
    	...
    
    def signup(request):
    	if request.user.is_authenticated:
    		return redirect('article:index')
    	...
    ```
    
3. `is_authenticated` 코드
    - 메서드가 아닌 속성 값임을 주의

## `login_required` 데코레이터

1. 설명 : 
    - 인증된 사용자에 대해서만 view 함수를 실행시키는 데코레이터
    - 비인증 사용자인 경우 `/accounts/login/` 주소로 redirect 시킴
        - 인증 앱 이름을 accounts 로 하면 좋은 이유!
2. 적용 :
    - 인증된 사용자만 게시글을 작성/수정/삭제 할 수 있도록 수정
    
    ```python
    # articles/views.py
    
    from django.contrib.auth.decorators import login_required
    
    @login_required
    def create(request):
    	...
    
    ...
    ```
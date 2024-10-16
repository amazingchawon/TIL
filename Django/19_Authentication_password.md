# 비밀번호 변경

1. 설명 : 인증된 사용자의 Session 데이터를 Update 하는 과정

## `PasswordChangeForm()`

1. 설명 : 비밀번호 변경시 사용자 입력 데이터를 받는 built-in **Form**
2. `form = PasswordChangeForm(request.user)` : 위치 인자 필요

## 실습

1. 비밀번호 변경 페이지 : Django는 비밀번호 변경 페이지를 회원정보 수정 form 하단에 별도 주소로 안내
    - `/user_pk/password/`
    - project의 url에서 작성해야 함
2. urls.py → Project의 urls.py
    
    ```python
    from accounts import views
    
    urlpatterns = [
    	...
     path('<int:user_pk>/password/', views.chage_password, name='change_password'),
    ]
    ```
    
3. views.py → app의 views.py
    
    ```python
    from django.contrib.auth.forms import PasswordChangeForm()
    
    def change_password(request, user_pk):
    	if request.method == 'POST':
    		form = PasswordChangeForm(request.user, request.POST)
    		if form.is_valid():
    			form.save()
    			return redirect('articles:index')
    	else:
    		form = PasswordChangeForm(request.user)
    	context = {
    		'form': form,
    	}
    	return render(request, 'accounts/change_password.html', context)
    ```
    
4. Template
    
    ```python
    <form action="{% url "change_password" user.pk %}" method-"POST">
    	{% csrf_token %}
    	{{ form.as_p }}
    	<input type="submit">
    </form>
    ```
    
    - `<form action="{% url -- %}">` :
        - 앱 네임이 없음 → project url이라서
        - user.pk 그냥 사용 가능

## 세션 무효화 방지

1. 암호 변경 시 세션 무효화
    - 비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어 버려 로그인 상태가 유지되지 못하고 로그아웃 처리됨
    - 비밀번호가 변경되면서 기존 세션과의 회원 인증 정보가 일치하지 않기 때문
    - 로그인 상태가 유지 되는 이유 : 클라이언트가 매 요청마다 세션 ID를 보내서
    - 비밀번호 변경 → 세션 정보 변경 → 세션 ID 변경
    - 하지만 클라이언트가 가지고 있는 세션 ID == 기존 세션 ID

### `update_session_auth_hash(request, user)`

1. 설명 :
    - 암호 변경 시 세션 무효화를 막아주는 함수
    - 암호가 변경되면 새로운 password의 Session Data로 기존 session을 자동으로 갱신

### 실습

1. views.py 수정
    
    ```python
    # accounts/views.py
    
    from django.contrib.auth import update_sessio_auth_hash
    
    def change_password(request, user_pk):
    	if request.method == 'POST':
    		form = PasswordChangeForm(request.user, request.POST)
    		if form.is_valid():
    			# save() 메소드가 user를 반환한다.
    			user = form.save()
    			update_session_auth_hash(request, user)
    			return redirect('articles:index')
    	else:
    		form = PasswordChangeForm(request.user)
    	context = {
    		'form': form,
    	}
    	return render(request, 'accounts/change_password.html', context)
    ```
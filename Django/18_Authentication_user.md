# 회원 가입

1. 설명 : User 객체를 Create 하는 과정

## UserCreationForm()

1. 설명 : 회원 가입시 사용자 입력 데이터를 받는 built-in **ModelForm**

## 실습

1. urls.py
    
    ```python
    # accounts/urls.py
    
    from django.urls import path
    from . import views
    
    app_name = 'accounts'
    urlpatterns = [
        ...
        path('signup/', views.signup, name='signup'),
    ]
    ```
    
2. views.py
    
    ```python
    # accounts/views.py
    from django.contrib.auth.forms import UserCreationForm
    
    def signup(request):
    		# create 
        if request.method == 'POST':
            form = UserCreationForm(request.POST)
            if form.is_valid():
                form.save()
                return redirect('articles:index')
        # new 로직
        else:
            form = UserCreationForm()
        context = {
            'form': form,
        }
        return render(request, 'accounts/signup.html', context)
    ```
    
3. Template
    
    ```html
    <!-- accounts/signup.html -->
    <h1>회원가입</h1>
    <form action="{% url "accounts:signup" %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
    ```
    
    - 이때 화면에 나오는 언어는 프로젝트의 settings.py에서 변경 가능

## 회원 가입 로직 에러

1. 오류 발생 : Manager isn’t available; ‘auth.User’ has been swapped for ‘accounts/User’
2. 발생 이유 : 회원 가입에 사용하는 `UserCreationForm`이 대체한 커스텀 유저 모델이 아닌, 과거 Django의 기본 유저 모델로 작성된 클래스이기 때문
3. 커스텀 유저 모델을 사용하려면 다시 작성해야 하는 Form
    - Usercreationform
    - UserChangeForm
4. `accounts/forms.py` 에서 UserCreationForm과 UserChangeForm 커스텀
    - Custom user model을 사용할 수 있도록 상속 후 일부분만 재작성
    
    ```python
    # accounts/forms.py
    
    from django.contrib.auth import get_user_model
    from django.contrib.auth.forms import UserCreationForm, UserChangeForm
    
    class CustomUserCreationForm(UserCreationForm):
    	class Meta(UserCreationForm.Meta):
    		model = get_user_model()
    
    class CustomUserChangeForm(UserChangeForm):
    	class Meta(UserChangeForm.Meta):
    		model = get_user_model()
    ```
    

### get_user_model()

1. 설명 : 현재 프로젝트에서 활성화된 사용자 모델(active user model)을 반환하는 함수

### User 모델을 직접 참조하지 않는 이유

1. `get_user_model()`을 사용해 User 모델을 참조하면 커스텀 User 모델을 자동으로 반환해주기 때문
2. Django는 필수적으로 `get_user_model()` 을 사용해 User 클래스를 참조해야 한다고 강조하고 있음

# 회원 탈퇴

1. 설명 : User 객체를 Delete 하는 과정
2. 로그아웃과 비슷
    - 차이 : 삭제 대상
        - 로그아웃 : 세션 삭제
        - 회원 탈퇴 : 회원 정보 삭제

## 실습

1. urls.py
    
    ```python
    urlpatterns = [
    	...
    	path('delete/', views.delete, name='delete'),
    ]
    ```
    
    - article delete와 차이 비교
        - article delete : variable routing 필요
        - variable routing을 하지 않은 이유 :
            - 원래 article의 형식을 따른다면, variable routing으로 얻은 id로 User 모델에서 누가 회원 탈퇴를 요청한건지 검색
            - 하지만 회원 탈퇴에서는 누가 요청한건지 User 모델에서 검색할 필요가 없음
            - 왜냐하면, request 객체에 요청을 보내는 user 정보가 들어있기 때문에
2. views.py
    
    ```python
    # accounts/views.py
    
    def delete(request):
    	user = request.user
    	user.delete()
    	# request.user.delete()
    	return redirect('articles:index')
    ```
    
3. Template : 
    - 회원가입은 a태그로 생성 → 페이지를 달라고 하는 거기 때문에
    - 회원탈퇴는 form태그로 생성 → Post 요청을 보내야하기 때문에
    
    ```python
    <!-- articles/index.html ->
    
    <a href={% url "accounts:signup %}>회원 가입</a>
    <form action = "{% url accounts:delete %}" ..
    ```
    

## 탈퇴와 함께 기존 사용자의 Session Data 삭제 방법

1. 탈퇴 ≠ 세션을 지우는 것
    - 단지, User 객체를 지우는 것 뿐
    - 세션을 지우는 건 logout
        - 나중에 시간이 지나면 사라지긴 하지만, 깔끔하게 지울 수 있음 → logout을 하면 됨
2. 설명 :
    
    ```python
    # accounts/views.py
    
    def delete(request):
    	requet.user.delete()
    	auth_logout(request)
    ```
    
    - 사용자 객체 삭제 이후 로그아웃 함수 호출
    - 탈퇴 후 로그아웃의 순서가 바뀌면 안됨
        - 먼저 로그아웃을 진행하게 되면 해당 요청 객체 정보가 없어지기 때문에 탈퇴에 필요한 유저 정보 또한 없어지기 때문
    

# 회원 정보 수정

1. 설명 : User 객체를 Update 하는 과정

## `UserChangeForm()`

1. 회원 정보 수정 시 사용자 입력 데이터를 받는 built-in **ModelForm**

## 실습

1. urls.py
    
    ```python
    path('update/', views.update, name='update')
    ```
    
2. views.py
    
    ```python
    # accounts/views.py
    from .forms import CustomUserChangeForm
    
    def update(request):
    	if request.methon == 'POST':
    		form = CustomUserChangeForm(request.POST, instance=request.user)
    		if form.is_vaild():
    			form.save()
    			return redirect('articles:index')
    	else:
    		form = CustomUserChangeForm(instance=request.user)
    	context = {
    		'form': form,
    	}
    	return render(request, 'accounts/update.html, context)
    ```
    
3. Template
    
    ```html
    <!-- accounts/index.html -->
    
    <a href="{% url 'accounts:update' %}">회원정보 수정</a>
    ```
    

## UserChangeForm 사용 시 문제

1. 문제점 : 
    - User 모델의 모든 정보들(fields)까지 모두 출력됨
    - 일반 사용자들이 접근해서는 안되는 정보는 출력하지 않도록 해야 함
2. 해결 방안 : forms.py에서 CustomUserChangeForm에서 출력 필드를 다시 조정하기
    
    ```python
    # accounts/forms.py
    
    class CustomUserChangeForm(UserChangeForm):
        class Meta(UserChangeForm.Meta):
            model = get_user_model()
            fields = ('first_name', 'last_name', 'email',)
    ```
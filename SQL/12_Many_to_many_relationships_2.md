# Many to Many Relationships

## 팔로우 기능 구현

### 프로필 페이지

1. url 작성
    
    ```python
    # accounts/urls.py
    
    urlpatterns = [
    	...
    	path('profile/<username>', views.profile, name='profile'),
    ]
    ```
    
    - profile을 넣는 이유 : 작성 순서 상, <username> 아래 있는 주소들은 다 username으로 간주되어서 구분을 위해 넣어줌 (혹은 제일 하단에 배치)
2. view 함수
    
    ```python
    # accounts/views.py
    
    from django.contrib.auth import get_user_model
    
    def profile(request, username):
    	# 어떤 유저의 프로필을 보여줄건지 유저를 조회 (username을 사용해서 조회)
    	User = get_user_model()
    	person = User.objects.get(username=username)
    	# person = get_user_model().obejects.get(username=username)
    	context = {
    		'person': person,
    	}
    	return render(request, 'accounts/profile.html', context)
    ```
    
3. template
    
    ```html
    <!-- accounts/profile.html -->
    
    <h1>{{ person.username }}님의 프로필
    
    <hr>
    
    <h2>{{ person.username }}가 작성한 게시글</h2>
    {% for article in person.article_set.all %}
     <div>{{ article.title }}</div>
    {% endfor %}
    
    <hr>
    
    <h2>{{ person.username }}가 작성한 댓글</h2>
    {% for comment in person.comment_set.all %}
     <div>{{ comment.content }}</div>
    {% endfor %}
    
    <hr>
    
    <h2>{{ person.username }}가 좋아요한 게시글</h2>
    {% for article in person.like_articles_set.all %}
     <div>{{ article.title }}</div>
    {% endfor %}
    ```
    
4. 프로필 페이지로 이동할 수 있는 링크 작성
    
    ```html
    <!-- articles/index.html -->
    
    <a href="{% url "accounts:profile" user.username %}">내 프로필</a>
    
    <a href="{% url "accounts:profile" article.user.username %}">
        <p>작성자: {{ article.user.username }}</p>
    </a>
    ```
    

### 모델 관계 설정

1. User(M) - User(N) :
    - 0명 이상의 회원은 0명 이상의 회원과 관련
    - 회원은 0명 이상의 팔로워를 가질 수 있고, 0명은 이상의 다른 회원들을 팔로잉 할 수 있음
2. ManyToManyField 작성
    
    ```python
    # accounts/models.py
    
    class User(AbstractUser):
    	followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
    ```
    
    - 참조 : 내가 팔로우하는 사람들 (팔로잉, following)
    - 역참조 : 상대방 입장에서 나는 팔로워 중 한명 (팔로워, followers)
    - 바뀌어도 상관 없으나 관계 조회 시 생각하기 편한 방향으로 정한 것
3. mirations 진행 후 중개 테이블 확인
    - 중개 테이블
        
        
        | id | from_user_id | to_user_id |
        | --- | --- | --- |
        |  |  |  |

### 기능 구현

1. url 작성
    
    ```python
    # accounts/urls.py
    
    urlpatterns = [
    	...,
    	path('<int:user_pk>/follow/', views.follow, name='follow'),
    ]
    ```
    
2. view 함수 작성
    
    ```python
    # accounts/views.py
    
    @login_required
    def follow(request, user_pk):
    	User = get_user_model()
    	person = User.objects.get(pk=user_pk)
        
      # 다른 사람만 팔로우 할 수 있게
    	if person != request.user:
    		if request.user in person.followers.all():
    			person.followers.remove(request.user)
    		else:
    			person.followers.add(request.user)
                # request.user.following.add(person)
    	return redirect('accounts:profile', person.username)
    ```
    
3. template 작성
    
    ```html
    <!-- accounts/profile.html -->
    
    <h1>{{ person.username }}님의 프로필
        {% if request.user != person %}
        <div>
            <form action="{% url "accounts:follow" person.pk %}" method="POST">
                {% csrf_token %}
                {% if request.user in person.followers.all %}
                    <input type="submit" value='언팔로우'>
                {% else %}
                    <input type="submit" value ='팔로우'>
                {% endif %}
            </form>
        </div>
        {% endif %}
        <hr>
    ```
    
4. 팔로잉 수 표시
    
    ```python
    <!-- accounts/profile.html -->
    <div>
    	팔로잉 : {{ person.followings.all|length}} / 팔로워 : {{ person.followers.all|length }}
    </div>
    ```
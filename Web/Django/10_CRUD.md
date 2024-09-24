# CRUD

1. 정의 : 소프트웨어가 가지는 기본적인 데이터 처리 기능
    - Create : 저장
    - Read : 조회
    - Update : 갱신
    - Delete : 삭제

## Create

1. 객체 생성
    - 방법 1 : class로부터 instance 생성
        - 인스턴스 변수를 활용하여 정보 저장
    - 방법 2 : class로부터 instance 생성, 인자를 넘겨주어 정보 저장
    - 방법 3 : QuerySet API 중 create() 매서드 활용
2. 저장
    - `인스턴스.save()` : 객체를 데이터베이스에 저장하는 인스턴스 메서드
3. Create 로직을 구현하기 위해 필요한 view 함수의 개수는? 2개
    - `new` : 사용자 입력 데이터를 받을 페이지를 렌더링
    - `create` : 사용자가 입력한 요청 데이터를 받아 DB에 저장
4. 사용자 입력 받기 (new 기능 구현)
    - STEP 1. url 등록
        
        ```python
        # articles/url.py
        
        urlpatterns = [
        	...
        	path('new/', views.new, name='new')
        ]
        ```
        
    - STEP 2. view
        
        ```python
        # articles/views.py
        
        def new(request):
        	return render(request, 'articles/new.html')
        ```
        
    - STEP 3. Template
        
        ```html
        <!-- templates/articles/new.html -->
        
        <h1>NEW</h1>
        <form action="{% url 'articles:create' %}" method="POST">
        	<div>
        		<lablel for="title">Title: </label>
        		<input type="text" name="title" id="title">
        	</div>
        	<div>
        		<label for="content">Content: </lable>
        		<textarea name="content" id="content"></textarea>
        	</div>>
        	<input type="submit">
        </form>
        <hr>
        <a href="{% url 'articles:index' %}">[back]</a>
        ```
        
5. 사용자 입력 데이터 DB에 저장
    - STEP 1. url
        
        ```python
        # articles/url.py
        
        urlpatterns = [
        	...
        	path('create/', views.create, name='create')
        ]
        ```
        
    - STEP 2. view
        
        ```python
        # articles/views.py
        # 과거 catch와 같은 기능
        form django.shortcuts import render, redirect
        
        def create(request):
            # 1. 사용자 요청으로부터 입력 데이터를 추출
            title = request.POST.get('title')
            content = request.POST.get('content')
            # 2. 데이터 저장
            # 3. 추출한 입력 데이터를 활용해 DB에 저장 요청
        
            article = Article(title = title, content = content)
            article.save()
        
            return render(request, 'articles/create.html')
        ```
        
        - 데이터 저장 방법
            - 방법 1 : 인스턴스 생성, 인스턴스 변수로 값 입력
                
                ```python
                # 저장 1 : 인스턴스 생성, 인스턴스 변수로 값 입력
                article = Article()
                article.title = title
                article.content = content
                article.save()
                ```
                
            - 방법 2 : 인스턴스에 초기값 넣어서 생성
                
                ```python
                # 저장 2 : 인스턴스에 초기값 넣어서 생성
                # 가장 바람직한 방법
                # 추후 유효성 검사에 가장 적합한 방법
                article = Article(title = title, content = content)
                article.save()
                ```
                
                - 가장 바람직한 방법 → 추후 유효성 검사에 가장 적합한 방법
            - 방법 3 : `create()` 메소드 사용
                
                ```python
                # 저장 3 : create() 메소드 사용
                # 유효성 검사는 저장 전에 일어남 -> save() 메소드를 사용하지 않기 때문에 검사 불가
                Article.objects.create(title=title, content=content)
                ```
                
                - 유효성 검사는 저장 전에 일어남 → `create()` 메소드 사용시,  `save()` 메소드를 사용하지 않기 때문에 검사 불가능
    - STEP 3. Template
        
        ```html
        <!-- templates/articles/create.html -->
        
        <h1>게시글이 작성 되었습니다</h1>
        ```
        

### Redirect

1. 해결해야 하는 문제 : 게시글 작성 후 완료를 알리는 페이지를 응답하는 흐름은 어색함
2. 서버는 데이터 저장 후 페이지를 응답하는 것이 아닌 사용자를 적절한 기존 페이지로 보내야 함
    - 사용자를 보낸다 = 사용자가 GET 요청을 한번 더 보내도록 해야 함
    - 실제로 서버가 클라이언트를 직접 다른 페이지로 보내는 것이 아닌 클라이언트가 GET 요청을 한번 더 보내도록 응답하는 것
3.  `redirect()` : 클라이언트가 인자에 작성된 주소로 다시 요청을 보내도록 하는 함수
    
    ```python
    # articles/views.py
    # 과거 catch와 같은 기능
    form django.shortcuts import render, redirect
    
    def create(request):
    	...
      # redirect()
      # return redirect('articles:index')
      return redirect('articles:detail', article.pk)
    ```
    
4. `redirect()` 동작 원리
    - `redirect` 응답을 받은 클라이언트는 `detail` url로 다시 요청을 보내게 됨
    - 결과적으로 `detail` view 함수가 호출되어 `detail` view 함수의 반환결과인 `detail.html` 페이지를 응답 받게 되는 것
    - 결국, 사용자는 게시글 작성 후 작성된 게시글의 `detail.html` 페이지로 이동하는 것처럼 느끼게 됨
    - 이 과정은 개발자 모드에서 확인 가능!

## Read

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
2. 실습 : 게시판의 게시글을 하나씩 볼 수 있도록 `detail.html` 파일 만들기
    - STEP 1. url 등록
        
        ```python
        # articles/urls.py -> 앱의 url 파일
        
        urlpatterns = [
        	...
        	path('<int:pk>/', views.detail, name='detail'),
        ]
        ```
        
        - `<int:pk>` : variable routing 적용, int형의 pk라는 이름을 가진 변수를 URL에 포함
    - STEP 2. view
        
        ```python
        # articles/views.py -> 앱의 view 파일
        
        def detail(request, pk):
        	# url로부터 전달받은 pk를 활용해 데이터를 조회
        	article = Article.objects.get(pk=pk)
        	context = {
        	 'article': article,
        	}
        	return render(request, 'articles/detail.html', context)
        ```
        
        - `Article.objects.get(pk=pk)`
            - `pk=pk` : 앞쪽의 pk는 키워드 인자 형식, 뒤쪽 pk는 매개변수 값
    - STEP 3. template 작성
        
        ```html
        <!-- templates/articles/detail.html -->
        
        <h1>Detail</h1>
            <h3>{{ article.pk }}번째 글</h3>
            <hr>
            <p>제목: {{ article.title }}</p>
            <p>내용: {{ article.content }}</p>
            <p>수정일: {{ article.updated_at }}</p>
            <p>저장일: {{ article.created_at }}</p>
            <hr>
            <!-- 삭제 버튼 -->
            <form action="{% url "articles:delete" article.pk %}" method="POST">
                {% csrf_token %}
                <input type="submit" value='삭제'>
            </form>
            <!-- 뒤로 가기 버튼 -->
            <a href="{% url "articles:index" %}">[back]</a>
        ```
        

## Update

1. 절차 및 메서드
    - 수정할 인스턴스 조회 → `get()`
    - 인스턴스 변수를 변경
    - 저장 → `save()`
2. Update 로직을 구현하기 위해 필요한 view 함수의 개수는? 2개
    - `edit` : 사용자 입력 데이터를 받을 페이지를 렌더링
    - `update` : 사용자가 입력한 데이터를 받아 DB에 저장
3. 게시글 수정 페이지 구현 (`edit` 기능 구현)
    - STEP 1. url
        
        ```python
        # articles/urls.py -> 앱의 url 파일
        
        urlpatterns = [
            ...
            path('<int:pk>/edit/', views.edit, name='edit'),
        ]
        ```
        
    - STEP 2. view
        
        ```python
        # articles/views.py -> 앱의 view 파일
        
        def edit(request, pk):
            # 1. 조회 : 어떤 게시글을 수정할지
            article = Article.objects.get(pk=pk)
            context = {
                'article' : article
            }
            return render(request, 'articles/edit.html', context)
        ```
        
    - STEP 3. Template
        
        ```html
        <h1>Edit</h1>
        <form action="{% url "articles:update" article.pk %}" method="POST">
            {% csrf_token %}
            <input type="text" name="title" value="{{ article.title }}">
            <textarea name="content" id="">{{ article.content }}</textarea>
            <input type="submit" value="수정">
        </form>
        ```
        
4. 게시글 수정 페이지 구현 (`edit` 기능 구현)
    - STEP 1. url
        
        ```python
        urlpatterns = [
            ...
            path('<int:pk>/update/', views.update, name='update'),
        ]
        ```
        
    - STEP 2. view
        
        ```python
        # articles/views.py -> 앱의 view 파일
        
        def update(request, pk):
            # 1. 조회 : 어떤 게시글을 수정할지
            article = Article.objects.get(pk=pk)
        
            # 2. 사용자로부터 받은 새로운 입력 데이터 추출
            title = request.POST.get('title')
            content = request.POST.get('content')
            # 3. 기존 게시글의 데이터를 사용자에게 받은 데이터로 새로 할당
            article.title = title
            article.content = content
            # 4. 저장
            article.save()
        
            return redirect('articles:detail', article.pk)
        ```
        
    - STEP 3. Template
        - 별도의 html 파일은 필요 없음 → 게시글 수정 페이지에 저장 버튼을 만들고, 그 버튼에 url을 연결해주면 됨

## Delete

1. 절차 및 메서드
    - 수정할 인스턴스 조회 → `get()`
    - 인스턴스 변수를 삭제 → `delete()`
        - 삭제된 객체가 반환
2. 참고
    - 삭제한 데이터는 더 이상 조회 할 수 없음
    - django는 지워진 데이터의 id는 재사용하지 않음
3. 실습
    - STEP 1. url
        
        ```python
        # articles/urls.py -> 앱의 url 파일
        
        urlpatterns = [
        	...
        	path('<int:pk>/delete', views.delete, name='delete'),
        ]
        ```
        
    - STEP 2. view
        
        ```python
        # articles/views.py -> 앱의 view 파일
        
        def delete(request, pk):
        	article = Article.objects.get(pk=pk)
        	article.delete()
        	return redirect('article:index')
        ```
        
    - STEP 3. template
        - 별도의 html 파일은 필요 없음 → 개별 게시글 페이지에 삭제 버튼을 만들고, 그 버튼에 url을 연결해주면 됨
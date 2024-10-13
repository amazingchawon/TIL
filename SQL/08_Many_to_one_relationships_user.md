# Many to one relationships - User

1. User와 다른 모델 간의 모델 관계 설정
    - User & Article
    - User & Comment

## User & Article

### 모델 관계 설정

1. 설명 : 0개 이상의 게시글은 1명의 회원에 의해 작성될 수 있다.
2. User 외래 키 정의
    
    ```python
    # articles/models.py
    
    from django.conf import settings
    
    class Article(models.Model):
    	user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    	title = models.CharField(max_length=10)'
    	content = models.TextField()
    	created_at = models.DateTimeField(auto_now_add=True)
    	updated_at = models.DateTimeField(auto_now=True)
    ```
    

### User 모델을 참조하는 2가지 방법

|  | get_user_model() | settings.AUTH_USER_MODEL |
| --- | --- | --- |
| 반환 값 | User Object (객체) | ‘accounts.User’ (문자열) |
| 사용 위치 | models.py가 아닌 다른 모든 위치 | models.py |
1. 사용 위치가 다른 이유 : django 프로젝트 ‘내부적인 구동 순서’와 ‘반환 값’에 따른 이유
    - 기억해야 할 것은 User 모델은 직접 참조하지 않는 다는 것

## 게시글 구현

### 게시글  CREATE

1. 기존 ArticleForm 출력 변화 확인
    - User 모델에 대한 외래 키 데이터 입력을 위해 불필요한 input이 출력됨
    - 해결 방법 :  ArticleForm 출력 필드 수정
        
        ```python
        # articles/forms.py
        
        class ArticleForm(forms.ModelForm):
        	class Meta:
        		model = Article
        		fields = ('title', 'content',)
        ```
        
2. 게시글 작성 시 에러 발생
    - `user_id` 필드 데이터가 누락되었기 때문 → 이 값은 사용자가 입력하는 것이 아닌, 게시글을 작성하는 작성자의 정보를 알아서 저장해주어야함
    - 해결 방법 : 게시글 작성 시 작성자 정보가 함게 저장될 수 있도록 save의 commit 옵션 활용
        
        ```python
        # articles/views.py
        
        @login_required
        def create(request):
        	if request.method == 'POST':
        		form = ArticleForm(request.POST)
        		if form.is_valid():
        			article = form.save(commit=False)
        			article.user = request.user
        			article.save()
        			return redirect('articles:detail', article.pk)
        		else:
        			...
        ```
        

### 게시글 READ

1. 각 게시글의 작성자 이름 출력
    - 게시글 목록 페이지
    
    ```python
    <!!-- articles/index.html -->
    
    {% for article in articles %}
     <p>작성자 : {{ article.user }}</p>
     ...
    ```
    
    - 게시글 상세 페이지
    
    ```python
    <!!-- articles/detail.html -->
    
    <h2>DETAIL</h2>
     <p>작성자 : {{ article.user }}</p>
     ...
    ```
    

### 게시글 UPDATE

1. 게시글 수정 요청 사용자와 게시글 작성 사용자를 비교하여 본인의 게시글만 수정 할 수 있도록 개선
    
    ```python
    # articles/views.py
    
    @login_required
    def update(request, pk):
    	article = Article.objects.get(pk=pk)
    	if request.user == article.user:
    		if request.method == 'POST':
    			form = ArticleForm(request.POST, instance=article)
    			if form.is_vaild():
    				form.save()
    				return redirect('articles:detail', articel.pk)
    			else:
    				form = ArticleForm(instance==article)
    	else:
    		return redirect('articles:index')
    	...
    ```
    
2. 해당 게시글의 작성자가 아니라면, 수정/삭제 버튼을 출력하지 않도록 하기
    
    ```python
    <!-- articles.detail.html -->
    
    {% if request.user == article.user %}
    	...
    ```
    

### 게시글 DELETE

1. 삭제를 요청하려는 사람과 게시글을 작성한 사람을 비교하여 본인의 게시글만 삭제 할 수 있도록 하기
    
    ```python
    # articles/views.py
    
    @login_required
    def delete(request, pk):
    	article = Article.objects.get(pk=pk)
    	if request.user == article.user:
    		article.delete()
    	return redirect('articles:index)
    ```
    

## Comment & User

### 모델 관계 설정

1. User 외래 키 정의
    
    ```python
    # articles/models.py
    
    from django.conf import settings
    
    class Comment(models.Model):
    	article = models.ForeignKey(Article, on_delete=models.CASCADE)
    	user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    	content = models.TextField()
    	created_at = models.DateTimeField(auto_now_add=True)
    	updated_at = models.DateTimeField(auto_now=True)
    ```
    

## 댓글 구현

### 댓글 CREATE

1. 댓글 작성 시 이전 게시글 작성 할 때와 동일한 에러 발생
    - 댓글의 user_id 필드 데이터가 누락되었기 떄문
    - 해결 방법 : 댓글 작성 시 작성자 정보가 함께 저장할 수 있도록 작성
        
        ```python
        # articles/views.py
        
        def comments_create(request, pk):
        	article = Article.objects.get(pk=pk)
        	comment_form = CommentForm(request.POST)
        	if comment_form.is_valid():
        		comment = comment_form .save(commit=False)
        		comment.article = article
        		# 이 부분!
        		comment.user = request.user
        		comment.save()
        		return redirect('articles.detail, article.pk)
        ```
        

### 댓글 READ

1. 댓글 출력 시 댓글 작성자와 함께 출력
    
    ```python
    <!-- articles/detail.html -->
    
    {% for comment in comments %}
    	 <li>
    		 {{ comment.user}} - {{ comment.content }}
    		 ...
    	</li>
    {% endfor %}
    ```
    

### 댓글 DELETE

1. 댓글 삭제 요청 사용자와 댓글 작성 사용자를 비교하여 본인의 댓글만 삭제 할 수 있도록 구현
    
    ```python
    # articles/views.py
    
    def comments_delete(request, article_pk, comment_pk):
    	comment = Comment.objects.get(pk = comment_pk)
    	if request.user == comment.user:
    		comment.delete()
    	return redirect('articles:detial', article_pk)
    ```
    
2. 해당 댓글의 작성자가 아니라면, 댓글 삭제 버튼을 출력하지 않도록 함
    
    ```python
    <!-- articles/detail.html -->
    
    {% for comment in comments %}
    	 <li>
    		 {{ comment.user}} - {{ comment.content }}
    		 <!-- 이부분 -->
    		 {% if request.user = = comment.user %}
    			 ...
    			{% endif %}
    	</li>
    {% endfor %}
    ```
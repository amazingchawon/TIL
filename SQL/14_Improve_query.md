# Improve query

1. 설명 :
    - Query 개선하기
    - 같은 결과를 얻기 위해 DB 측에 보내는 query 개수를 점차 줄여 조회하기
2. 사전 준비
    - fixtures 데이터
        - 게시글 10개, 댓글 100개, 유저 5개
    - 모델 관계
        - N:1 → Article:User / Comment:Aritlce / Comment:Aritlce
        - N:M → Aricle:User
    
    ```python
    $ python manage.py migrate
    $ python manage.py loaddata users.json articles.json comments.json
    
    Installed 115 object(s) from 3 fixture(s)
    ```
    
    - 서버 실행 후 확인

## annotate

1. 설명 :
    - SQL의 GROUP BY를 사용
    - **쿼리셋의 각 객체에 계산된 필드**를 추가
    - 집계 함수(Count, Sum 등)와 함께 자주 사용됨
2. 예시 :
    
    ```python
    q = Book.objects.annotate(num_authors=Count('authors').order_by('-pk')
    ```
    
    - 의미
        - 결과 객체에 ‘num_authors’라는 새로운 필드 추가
        - 이 필드는 책과 연관된 저자의 수를 계산
    - 결과
        - 결과에는 기존 필드와 함께 ‘num_authors’ 필드를 가지게 됨
            - q.num_authors 로 접근 가능
        - book.num_authors로 해당 책의 저자 수에 접근할 수 있게 됨
3. 실습 :
    - 문제 원인 : 각 게시글마다 댓글 개수를 반복 평가
        
        ```html
        <!-- index_1.html -->
        
        <p> 댓글 개수 : {{ article.comment_set.count }}</p>
        ```
        
    - 문제 해결 : 게시글을 조회하면서 댓글 개수까지 한번에 조회해서 가져오기
        
        ```python
        # views.py
        
        def index_1(request):
        	article = Article.objects.annotate(Count('comment')).order_by('-pk')
        	context = {
        		'article':article,
        	}
        	return render(request, 'articles/index_1.html', context)
        ```
        
        ```html
        <!-- index_1.html -->
        
        <p> 댓글 개수 : {{ article.comment__count }}</p>
        ```
        
        - 여기서 -pk : DESC 방향을 뜻함

## select_related

1. 설명 :
    - SQL의 INNER JOIN를 사용
    - **1:1 또는 N:1 참조관계**에서 사용
        - ForeignKey나 OneToOneField 관계에 대해 JOIN을 수행
    - 단일 쿼리로 관련 객체를 함께 가져와 성능을 향상
2. 예시
    
    ```python
    Book.objects.select_related('publisher')
    ```
    
    - 의미
        - Book 모델과 연관된 Publisher 모델의 데이터를 함께 가져옴
        - ForeignKey 관계인 ‘publisher’를 JOIN하여 단일 쿼리 만으로 데이터를 조회
    - 결과
        - Book 객체를 조회할 때 연관된 Publisher 정보도 함께 로드
        - book.publisher.name과 같은 접근이 추가적인 데이터베이스 쿼리 없이 가능
3. 실습 :
    - 문제 원인 : 각 게시글마다 작성한 유저명까지 반복 평가
        
        ```python
        <!-- index_2.html -->
        
        {% for article in articles %}
        	<h3>작성자 : {{ article.user.username }}</h3>
        	<p>제목 : {{ article.title }}</p>
        	<hr>
        {% endfor %}
        ```
        
    - 문제 해결 : 게시글을 조회하면서 유저 정보까지 한번에 조회해서 가ㅑ져오기
        
        ```python
        # views.py
        
        def index_2(request):
        	articles = Article.objects.select_related('user').order_by('-pk')
        	context = {
        		'articles': articles,
        	}
        	return render(request, 'articles/index_2.html', context)
        ```
        

## prefetch_related

1. 설명 :
    - SQL이 아닌 Python을 사용한 JOIN 진행
        - 관련 객체들을 미리 가져와 메모리에 저장하여 성능을 향상
    - **M:N 또는 N:1 역참조 관계**에서 사용
        - ManyToManyField나 역참조 관계에 대해 별도의 쿼리를 진행
2. 예시 :
    
    ```python
    Book.objects.prefetch_related('authors')
    ```
    
    - 의미 :
        - Book과 Author는 ManyToMany 관계로 가정
        - Book 모델과 연관된 모든 Author 모델의 데이터를 미리 가져옴
        - Django가 별도의 쿼리로 Author 데이터를 가져와 관계를 설정
    - 결과 :
        - Book 객체들을 조회한 후, 연관된 모든 Author 정보가 미리 로드 됨
        - for author in book.authors.all()와 같은 반복이 추가적인 데이터베이스 쿼리 없이 실행됨
3. 실습 :
    - 문제 원인 : 각 게시글 출력 후 각 게시글의 댓글 목록까지 개별적으로 모두 평가
        
        ```python
        <!-- index_3.html -->
        
        {% for article in articles %}
        	<p>제목 : {{ article.title }}</p>
        	<p>댓글 목록</p>
        	{% for comment in article.comment_set.alll %}
        		<p>{{ comment.content }}</p>
        	{% endfor %}
        	<hr>
        {% endfor %}
        ```
        
    - 문제 해결 : 게시글을 조회하면서 참조된 댓글까지 한번에 조회해서 가져오기
        
        ```python
        # views.py
        
        def index_3(request):
        	article = Article.objects.prefetch_related('comment_set').order_by('-pk')
        	context = {
        		'articles': articles,
        	}
        	return render(request, 'articles/index_3.html', context)
        ```
        

## select_related & prefetch_related

1. 실습 :
    - 문제 원인 : 게시글 + 각 게시글의 댓글 목록 + 댓글의 작성자를 단계적으로 평가
        
        ```python
        <!-- index_4.html -->
        
        {% for article in articles %}
          <p>제목 : {{ article.title }}</p>
          <p>댓글 목록</p>
          {% for comment in article.comment_set.all %}
            <p>{{ comment.user.username }} : {{ comment.content }}</p>
          {% endfor %}
          <hr>
        {% endfor %}
        ```
        
    - 문제 해결 1단계 : 게시글을 조회하면서 참조된 댓글까지 한번에 조회
        
        ```python
        # views.py
        def index_4(request):
            articles = Article.objects.prefetch_related('comment_set').order_by('-pk')
            context = {
                'articles': articles,
            }
            return render(request, 'articles/index_4.html', context)
        ```
        
    - 문제 해결 2단계 : 게시글 + 각 게시글의 댓글 목록 + 댓글의 작성자를 한번에 조회
        
        ```python
        # views.py
        from django.db.models import Prefetch
        
        def index_4(request):
            articles = Article.objects.prefetch_related(
                Prefetch('comment_set', queryset=Comment.objects.select_related('user'))
            ).order_by('-pk')
        
            context = {
                'articles': articles,
            }
            return render(request, 'articles/index_4.html', context)
        ```
        

## 참고

### exists method

1. 설명 :
    - QuerySet에 결과가 하나 이상 존재하는지 여부를 확인하는 메서드
    - 결과가 포함되어 있으면 True를 반환하고, 포함되어있지 않으면 False를 반환
2. 특징 :
    - 데이터베이스에 최소한의 쿼리만 실행하여 효울적
    - 전체 QuerySet을 평가하지 않고 결과의 존재 여부만 확인
    - 대량의 QuerySet에 있는 특정 객체 검색에 유용
3. 예시
    
    ```python
    # articles/views.py
    
    @login_required
    def likes(request, article_pk):
    	article = Article.objects.get(pk=article_pk)
    	if request.user in article.like_users.all():
    		...
    ---
    
    @login_required
    def likes(request, article_pk):
    	article = Article.objects.get(pk=article_pk)
    	if article.like_users.filter(pk=request.user.pk).exists():
    		...
    ```
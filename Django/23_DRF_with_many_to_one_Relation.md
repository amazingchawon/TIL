# DRF with N:1 Relation

1. 실습 준비
    - Comment 클래스 정의
    - Migration 및 fixtures 데이터 로드

## URL 및 HTTP request method 구성

| URL | GET | POST | PUT | DELETE |
| --- | --- | --- | --- | --- |
| comments/ | 댓글 목록 조회 |  |  |  |
| comments/1/ | 단일 댓글 조회 |  | 단일 댓글 수정 | 단일 댓글 삭제 |
| articles/1/comments/ |  | 댓글 생성 |  |  |
- 댓글 생성의 경우, article의 주소가 필요하므로 새로운 URL이 필요

## GET method

### 댓글 목록 조회

1. 댓글 목록 조회를 위한 CommentSerializer 정의
    
    ```python
    # articles/serializers.py
    
    from .models import Article, Comment
    
    class CommentSerializer(serializers.ModelSerializer):
        class Meta:
            model = Comment
            fields = '__all__'
    ```
    
2. url 작성
    
    ```python
    # articles/serializers.py
    
    from .models import Article, Comment
    
    class CommentSerializer(serializers.ModelSerializer):
        class Meta:
            model = Comment
            fields = '__all__'
    ```
    
3. view 함수 작성
    
    ```python
    from .models import Article, Comment
    from .serializers import ArticleListSerializer, ArticleSerializer, CommentSerializer
    
    @api_view(['GET'])
    def comment_list(request):
        if request.method == 'GET':
            # 댓글 전체 조회
            comments = Comment.objects.all()
            # 댓글 목록 시리얼라이져 직렬화 진행
            serializer = CommentSerializer(comments, many=True)
            return Response(serializer.data)
    ```
    

### 단일 댓글 조회

1. url 작성
    
    ```python
    urlpatterns = [
        ...
        path('comments/<int:comment_pk>/', views.comment_detail),
    ]
    ```
    
2. view 함수 작성
    
    ```python
    @api_view(['GET'])
    def comment_detail(request, comment_pk):
        # 단일 댓글 조회
        comment = Comment.objects.get(pk=comment_pk)
        if request.method == 'GET':
            # 단일 댓글 시리얼라이져 직렬화 진행
            serializer = CommentSerializer(comment)
            return Response(serializer.data)
    ```
    

## POST method

### 단일 댓글 생성

1. url 작성
    
    ```python
    urlpatterns = [
        ...
        path('articles/<int:article_pk>/comments/', views.comment_create),
    ]
    ```
    
2. view 함수 작성
    
    ```python
    @api_view(['POST'])
    def comment_create(request, article_pk):
        # 게시글 조회 : 어떤 게시글에 작성되는 댓글인지
        article = Article.objects.get(pk=article_pk)
        # 사용자 입력 데이터를 직렬화 : 사용자가 입력한 댓글 데이터
        serializer = CommentSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            # 외래키 데이터 입력 후 저장
            serializer.save(article=article)
            return Response(serializer.data, status=status.HTTP_201_CREATED)
    ```
    
3. 에러 처리 : 400
    
    ```python
    # articles/serializers.py
    
    from .models import Article, Comment
    
    class CommentSerializer(serializers.ModelSerializer):
        class Meta:
            model = Comment
            # fields에 작성된 필드는 모두 유효성 검사 목록에 포함됨
            fields = '__all__'
            # 외래 키 데이터를 읽기 전용 필드로 지정
            # 외래 키 데이터는
            # 1. 유효성 검사에서는 제외
            # 2. 결과 데이터에는 포함하고 싶음
            read_only_fields = ('article',)
    ```
    
    - CommentSerializer에서 외래 키에 해당하는 article field 또한 사용자로부터 입력 받도록 설정되어 있기 때문에 서버 측에서는 누락되었다고 판단한 것
    - 유효성 검사 목록에서 제외 필요
        - 하지만, 결과 데이터에는 포함을 원함
    - article field를 **읽기 전용 필드**로 설정하기

## DELETE & PUT method

### 단일 댓글 삭제 및 수정

1. view 함수 작성
    
    ```python
    @api_view(['GET', 'DELETE', 'PUT'])
    def comment_detail(request, comment_pk):
        ...
        elif request.method == 'DELETE':
            comment.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
        
        elif request.method == 'PUT':
            serializer = CommentSerializer(comment, data=request.data)
            if serializer.is_valid(raise_exception=True):
                serializer.save()
                return Response(serializer.data)
    ```
    
    - 수정 시에 article pk를 넘겨야 할까?
        - no, 이미 외래키 정보가 저장되어있는 데이터를 수정하는 것이기 때문

## 응답 데이터 재구성

### 댓글 조회 시 게시글 출력 내역 변경

1. 목표 : 댓글 조회 시 게시글 번호만 제공해주는 것이 아닌 ‘게시글의 제목’까지 제공하기
2. Serializer 내부에 추가 선언
    
    ```python
    class CommentSerializer(serializers.ModelSerializer):
        class ArticleTitleSerializer(serializers.ModelSerializer):
            class Meta:
                model = Article
                fields = ('title',)
        
        # 기존 article 데이터 값을 override
        # 문제 : 기존 필드를 override 하게 되면 Meta 클래스의 read_only_fields를 사용할 수 없음
        # 해결  : 모델 시리얼라이져의 read_only 인자 값으로 재설정
        article = ArticleTitleSerializer(read_only=True)
        
        class Meta:
            model = Comment
            # fields에 작성된 필드는 모두 유효성 검사 목록에 포함됨
            fields = '__all__'
            # 외래 키 데이터를 읽기 전용 필드로 지정
            # 외래 키 데이터는
            # 1. 유효성 검사에서는 제외
            # 2. 결과 데이터에는 포함하고 싶음
            read_only_fields = ('article',)
    ```
    

# 역참조 데이터 구성

1. 목표 : Article → Comment 간 역참조 관계를 활용한 JSON 데이터 재구성
    - 단일 게시글 조회 시 **해당 게시글에 작성된 목록**도 함께 붙여서 응답
    - 단일 게시글 조회 시 **해당 게시글에 작성된 댓글 개수**도 함께 붙여서 응답

## 단일 게시글 + 댓글 목록

### Nested relationship 역참조 매니저 활용

1. 설명 : 
    - 모델 관계 상으로 참조하는 대상은 참조되는 대상의 표현에 포함되거나 중첩될 수 있음
        - 실습에서 참조하는 대상 : Comment
        - 실습에서 참조되는 대상 : Article
    - 이러한 중첩된 관계는 serializers를 필드로 사용해서 표현 가능
2. Serializer
    
    ```python
    class ArticleSerializer(serializers.ModelSerializer):
        class CommentDetailSerializer(serializers.ModelSerializer):
            class Meta:
                model = Comment
                fields = ('id', 'content',)
        
        # 기존에 존재하는 comment_set 역참조 데이터를 override
        # 새로운 fields가 생긴 셈 -> 그래서 유효성 검사할 때 포함이 안되도록
        # Article과 Comment는 1:N 관계, 따라서 comment 데이터는 쿼리셋 -> many 옵션을 넣어주어야 함
        comment_set = CommentDetailSerializer(read_only=True, many=True)
        
        class Meta:
            model = Article
            fields = '__all__
    ```
    
3. 역참조 매니저 이름 변경하는 방법
    - models.py에서 역참조 매니저 이름 설정
    
    ```python
    # articles/models.py
    
    class Comment(models.Model):
    	# 여기에 related name 추가
        article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name=comments)
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    ```
    
    - serializers.py에서 역참조 매니저 이름 변경
    
    ```python
    class ArticleSerializer(serializers.ModelSerializer):
        class CommentDetailSerializer(serializers.ModelSerializer):
            class Meta:
                model = Comment
                fields = ('id', 'content',)
        
        # 여기
        comments = CommentDetailSerializer(read_only=True, many=True)
        
        class Meta:
            model = Article
            fields = '__all__
    ```
    

## 단일 게시글 + 댓글 개수

1. 댓글 개수에 해당하는 새로운 필드 생성
    - 어떤 필드를 가져올지는 공식문서를 참고
    
    ```python
    # articles/serializers
    
    class ArticleSerializer(serializers.ModelSerializer):
    	...
    	comment_set = CommentSerializer(many=True, read_only=True)
    	# 댓글 개수 제공을 위한 새로운 필드를 생성
    	comment_count = serializers.IntegerField(source="comment_set.count', read_only='True')
    ```
    

### `source` arguments

1. 설명 :
    - 필드를 채우는 데 사용할 속성의 이름
    - 점 표기법 (dotted notation)을 사용하여 속성을 탐색할 수 있음
    - 문자열로 작성, 인스턴스 생략
2. 특징 :
    - 기존 문법 : ariticle.comment_set.count()
        - 이미 애초에 Article로 만들어진 model serializer이라 필요 없음

## 읽기 전용 필드

1. 설명 :
    - 데이터를 전송 받은 시점에서 유효성 검사에서 제외시키고, 데이터 조회 시에는 출력하는 필드
2. 사용하는 이유 :
    - 원인 : 특정 필드를 override 혹은 추가한 경우 Meta class의 `read_only_fields` 는 동작하지 않음
        - 사용자에게 입력으로는 받지 않지만 제공은 해야하는 경우
        - 새로운 필드 값을 만들어 제공해야 하는 경우
    - 해결 : 이러한 경우 새로운 필드에 `read_only` 키워드 인자로 작성해야함
3. 특징 및 주의사항
    - 유효성 검사에서 제외됨
    - 단, 유효성 검사에서 제외된다고 해서 반드시 생성 로직에서만 사용이 국한되는 것은 아님

### read_only_fields 속성과 read_only 인자의 사용처

1. read_only_fields :
    - 기존 외래 키 필드 값을 그대로 응답 데이터에 제공하기 위해 지정하는 경우
2. read_only :
    - 기존 외래 키 필드 값의 결과를 다른 값으로 덮어 쓰는 경우
    - 새로운 응답 데이터 값을 제공하는 경우

# 참고

## 올바르게 404 응답하기

1. 현재 코드의 문제점
    - 존재하지 않은 게시글을 요청했을 때 500번이 뜸
        - 클라이언트의 요청이 잘못 된 것이므로 404를 띄워야 함
2. 문제의 원인
    - 게시글을 가져오는 get 메서드의 특징 때문
        - 조회 한 객체가 없을 때 DoesNotExist 예외를 발생
        - 조회한 객체가 2개 이상일 때 예외 발생
    - 서버는 예외가 발생하면 코드가 중단됨 → 500 에러
3. 해결책
    - 예외 처리 (try-except)

### Django shortcut functions

1. render()
2. redirect()
3. get_object_or_404() :
    - 모델 manager objects에서 get()을 호출하지만, 해당 객체가 없을 땐 기존 DoesNotExist 예외 대신 Http404를 raise 함
4. get_list_or_404() :
    - 모델 manager objects에서 filter()의 결과를 반환하고, 해당 객체 목록이 없을 땐 Http404를 raise 함
5. 왜 사용해야 할까?
    - 클라이언트에게 “서버에 오류가 발생하여 요청을 수행할 수 없다(500)”라는 원인이 정확하지 않은 에러를 제공하기 보다는, 적절한 예외 처리를 통해 클라이언트에게 보다 정확한 에러 현황을 전달하는 것도 매우 중요한 개발 요소 중 하나이기 때문

## 복잡한 ORM 활용 시 권장 방식

1. 복잡한 query나 로직은 View 함수에서 진행
    - 여러 모델을 조인하거나 복잡한 집계가 필요한 경우 View 함수에서 처리
    - 필요한 경우 View 함수에서 select_related()나 prefetch_related()를 사용하여 query를 최적화
# DRF Django REST Framework

1. 설명 : Django에서 Restful API 서버를 쉽게 구축할 수 있도록 도와주는 오픈소스 라이브러리

# DRF with Single Model

1. 프로젝트 준비
    - 패키지 설치 : `pip install djangorestframework`
    - INSTALLED APP 에 ‘rest_framework’ 추가
2. Postman

## Serializer

1. Serialization 직렬화
    - 설명 :
        - 여러 시스템에서 활용하기 위해 데이터 구조나 객체 상태를 나중에 재구성할 수 있는 포맷으로 변환하는 과정
        - 어떠한 언어나 환경에서도 나중에 다시 쉽게 사용할 수 있는 포맷으로 변환하는 과정
2. Serializer :
    - Serialization을 진행하여 Serialized data를 반환해주는 클래스

### ModelSerializer

1. 설명 :
    - Django 모델과 연결된 Serializer 클래스
    - 일반 Serializer과 달리 사용자 입력 데이터를 받아 자동으로 모델 필드에 맞추어 Serialization을 진행
2. ModelSerializer class 사용 예시
    - Article 모델을 토대로 직렬화를 수행하는 ArticleSerializer 정의
        - 게시글 데이터 목록 제공
            
            ```python
            # articles/serializers.py
            
            from rest_framework import serializers
            from .models import Article
            
            class ArticleSerializer(serializers.Modelserializer):
            	class Meta:
            		model = Article
            		fields = '__all__'
            ```
            
        - serializers.py의 위치나 파일명은 자유롭게 작성 가능

## CRUD with ModelSerializer

1. URL과 HTTP requests methods 설계
    
    
    |  | GET | POST | PUT | DELETE |
    | --- | --- | --- | --- | --- |
    | aritcles/ | 전체 글 조회 | 글 작성 |  |  |
    | articles/1/ | 1번 글 조회 |  | 1번 글 수정 | 1번 글 삭제 |

## GET method : 조회

### 게시글 데이터 목록 조회하기

1. Serializer
    
    ```python
    # articles/serializer.py
    
    from rest_framework import serializers
    from .models import Article
    
    class AritlceListSerializer(serializers.ModelSerializer):
        class Meta:
            model = Article
            fields = ('id', 'title', 'content',)
    ```
    
2. url 및 view 함수
    
    ```python
    # articles/urls.py
    
    urlpatterns = [
        path('articles/', views.article_list),
    ]
    ```
    
    ```python
    # articles/views.py
    
    from rest_framework.response import Response
    from rest_framework.decorators import api_view
    
    from .models import Article
    from .serializers import AritlceListSerializer
    
    @api_view(['GET'])
    def article_list(request):
        # 전체 게시글 조회 (타입: 쿼리셋, 그런데 쿼리셋은 장고에서 )
        articles = Article.objects.all()
        # 변환하기 쉬운 포멧으로 전환 (직렬화)
        # 다중 데이터는 many 인자 넣어주기
        serializer = AritlceListSerializer(articles, many=True)
        return Response(serializer.data)
    ```
    
    - `many` 옵션 : Serialize 대상이 QuerySet인 경우 입력
    - `data` 속성 : Serialized data 객체에서 실제 데이터를 추출
    - `api_view` decorator :
        - DRF view 함수에서는 필수로 작성되며 view 함수를 실행하기 전 HTTP 메서드를 확인
        - 기본적으로 GET 메서드만 허용되며 다른 메서드 요청에 대해서는 405 Method Not Allowed로 응답
        - DRF view 함수가 응답해야 하는 HTTP 메서드 목록을 작성
3. 과거 view 함수와의 응답 데이터 비교
    - 똑같은 데이터를
    - 과거 : HTML에 출력되도록 페이지와 함께 응답했던 view 함수
    - 현재 : JSON 데이터로 serialization 하여 페이지 없이 응답하는 view 함수

### 단일 게시글 데이터 조회하기

1. 각 게시글의 상세 정보를 제공하는 ArticleSerializer 정의
    
    ```python
    # articles/serializer.py
    
    class ArticleSerializer(serializers.ModelSerializer):
        class Meta:
            model = Article
            fields = '__all__'
    ```
    
2. url 및 view 함수 작성
    
    ```python
    # articles/urls.py
    
    urlpatterns = [
        path('articles/<int:article_pk>/', views.article_detail),
    ]
    ```
    
    ```python
    # articles/views.py
    
    @api_view(['GET'])
    def article_detail(request, article_pk):
        # 단일 게시글 조회
        article = Article.objects.get(pk=article_pk)
        # 직렬화 진행
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    ```
    

## POST method : 생성

### 게시글 데이터 생성하기

1. 설명 :
    - 데이터 생성이 성공했을 경우 201 Created 응답
    - 데이터 생성이 실패 했을 경우 400 Bad request 응답
2. view 함수
    
    ```python
    from rest_framework import status
    
    @api_view(['GET', 'POST'])
    def article_list(request):
    		if request.method == 'GET':
    		...
        elif request.method == 'POST':
            # 모델 시리얼라이저를 사용해서 사용자 입력 데이터를 받아 직렬화를 진행
            serializer = ArticleSerializer(data=request.data)
            # 유효성 검사
            if serializer.is_valid():
                serializer.save()
                # 저장 성공 후 201 응답 상태코드를 반환
                return Response(serializer.data, status=status.HTTP_201_CREATED)
            # 유효성 검사 실패 후 400 응답 상태코드를 반환
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
    ```
    

## DELETE method : 삭제

1. 게시글 데이터 삭제하기
    - 요청에 대한 데이터 삭제가 성공했을 경우는 204 No Content 응답
2. view 함수
    
    ```python
    @api_view(['GET', 'DELETE'])
    def article_detail(request, article_pk):
        # 단일 게시글 조회
        article = Article.objects.get(pk=article_pk)
    
        if request.method == 'GET':
            # 직렬화 진행
            serializer = ArticleSerializer(article)
            return Response(serializer.data)
    
        elif request.method == 'DELETE':
            article.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
    ```
    

## PUT method : 수정

1. 게시글 데이터 수정하기
    - 요청에 대한 데이터 수정이 성공했을 경우 200 OK 응답
2. view 함수
    
    ```python
    @api_view(['GET', 'DELETE', 'PUT'])
    def article_detail(request, article_pk):
        ...    
        elif request.method == 'PUT':
            serializer = ArticleSerializer(article, data=request.data, partial=True)
            if serializer.is_valid():
                serializer.save()
                # status 200이 기본이라 안적어주어도 됨
                return Response(serializer.data) 
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
    ```
    

### `partial` argument

1. 설명 :
    - 부분 업데이트를 허용하기 위한 인자
        - 예를 들어, partial 인자 값이 False 일 경우 게시글 title만을 수정하려고 해도 반드시 content 값도 요청 시 함께 전송해야 함
    - 기본적으로 serializer는 모든 필수 필드에 대한 값을 전달 받기 때문
        - 즉, 수정하지 않는 다른 필드 데이터도 모두 전송해야 하며 그렇지 않으면 유효성 검사에서 오류가 발생

### raise_exception

1. 설명 :
    - is_valid()의 선택인자
    - 유효성 검사를 통과하지 못할 경우 ValidationError 예외를 발생시킴
    - DRF에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며 기본적으로 HTTP 400응답을 반환
2. 코드
    
    ```python
    # articles/views.py
    
    @api_view(['GET', 'POST'])
    def article_list(request):
        ...
        elif request.method == 'POST':
            serializer = ArticleSerializer(data=request.data)
            # 여기
            if serializer.is_valid(raise_exception=True):
                serializer.save()
                return Response(serializer.data, status=status.HTTP_201_CREATED)
            # 여기 주석 가능
            # return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
    ```
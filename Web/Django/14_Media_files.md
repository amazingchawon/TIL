# Media Files

1. 정의 : 사용자가 웹에서 업로드하는 정적 파일 (user-uploaded)

## 미디어 파일을 제공하기 전 준비사항

1. `settings.py`에 `MEDIA_ROOT`, `MEDIA_URL` 설정
    
    ```python
    # settings.py
    
    # 여기서 'media'는 예시
    MEDIA_ROOT = BASE_DIR / 'media'
    
    MEDIA_URL = 'media/'
    ```
    
    - `MEDIA_ROOT`
        - 미디어 파일들이 위치하는 디렉토리의 절대 경로
        - 실제 해당 파일의 업로드가 끝나면 파일이 저장되는 경로
    - `MEDIA_URL`
        - `MEDIA_ROOT`에서 제공되는 미디어 파일에 대한 주소를 생성 (STATIC_URL과 동일한 역할)
2. `MEDIA_ROOT`와 `MEDIA_URL`에 대한 URL 지정
    
    ```python
    # crud/urls.py -> 프로젝트의 urls.py
    
    from django.conf import settings
    from django.conf.urls.static import static
    
    urlpattterns = [
    	...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    # + static(URL 주소, 미디어 파일의 실제 위치)
    ```
    

## 이미지 업로드

1. `ImageField()`
    - 이미지 업로드에 사용하는 모델 필드
    - 이미지 객체가 직접 DB에 저장되는 것이 아닌 **이미지 파일의 경로** 문자열이 저장됨
        - 타고타고 올라가다보면 `ImageField()`는 문자열 필드를 상속받음
2. STEP 1. `models.py`에 `ImageField()` 추가
    - 주의 : Model 필드는 기본적으로 공백을 허용하지 않음
        - 다른 조건을 설정하지 않고 그냥 ImageField를 추가한다면 → 모든 사용자는 이미지를 업로드 해야함
        - 따라서 공백이 허용되게 조건을 추가해 주어야함
            - `models.ImageField(blank=True)`
    - 에러 발생 : 공백을 허용하게 하고 저장을 하면, 라이브러리가 없다고 경고가 뜸
        - `pip install Pillow`
    - migration 진행
3. STEP 2. `form` 태그 수정
    - `form` 태그는 기본적으로 텍스트만 전달 → `enctype` 속성을 통해 데이터 전송 방식을 변경해주어야 함
        - `<form action="" method="POST" enctype="multipart/form-data">`
            - `multipart/form-data` : 모든 문자를 인코딩하지 않음을 명시. 이 방식은 요소가 파일이나 이미지를 서버로 전송할 때 주로 사용
4. STEP 3. View 수정
    
    ```python
    # articles/views.py
    
    def create(request):
    	if request.method == 'POST':
    		form = ArticleForm(request.POST, request.FILES)
    ```
    
    - 어떤 View 함수?
        - form 에서 action을 어디로 보내는 지 확인
        - 해당  url이 어떤 view 함수를 호출하는지 확인
    - 해당 view 함수에서 ModelForm의 객체를 생성할때 2번째 인자 자리에 요청 받은 파일 데이터 작성
        - 왜 2번째 자리? ModelForm의 상위 클래스 BaseModelForm의 생성자 함수의 2번째 위치 인자로 파일을 받도록 설정되어 있음

## 업로드 이미지 제공하기

1. 방법 : DTL의 `url` 속성을 통해 업로드 파일의 경로 값을 얻을 수 있음
    - `article.image.url` : 업로드 파일의 경로
    - `article.image` : 업로드 파일의 파일 이름
2. 실습
    
    ```html
    <!-- articles/detail.html> 게시글 상세 페이지 template -->
    
    <img src="{{ article.image.url }}" alt="img">
    ```
    

## 업로드 이미지 수정하기

1. 방법
    - 수정 페이지 form 요소에 enctype 속성 추가
    - view 함수에서 업로드 파일에대한 추가 코드 작성 (request.FILES)

## Media 파일 추가 경로

1. `upload_to` argument : `ImageField()`의 upload_to 속성을 사용하여 다양한 추가 경로 설정
    
    ```python
    # Models.py
    
    # 1. 기본 경로 설정
    image = models.ImageField(blank=True, upload_to='images/')
    
    # 2. 업로드 날짜로 경로 설정
    image = models.ImageField(blank=True, upload_to='%Y/%m/%d')
    
    # 3. 함수 형식으로 경로 설정
    def articles_imgae-path(instance, filename):
    	return f'images/{instance.user.username}/{filename}'
    
    image = models.ImageField(blank=True, upload_to=articles_image_path)
    ```
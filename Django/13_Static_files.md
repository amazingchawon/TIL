# Static files

정적 파일을 제공하려면 요청에 응답하기 위한 URL이 필요

1. 정의
    - 서버 측에서 변경되지 않고 고정적으로 제공되는 파일 (이미지, JS, CSS 파일 등)
    - 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 응답하면 되는 파일
2. 웹 서버와 정적 파일
    - 웹 서버의 기본동작 : 특정 위치(URL)에 있는 자원을 요청(HTTP request) 받아서 응답(HTtp ersponse)을 처리하고 제공하는 것
    - 웹 서버는 요청 받은 URL로 서버에 존재하는 정적 자원을 제공함
    - 따라서, 정적 파일을 제공하기 위한 경로(URL)가 있어야 함

## Static files 경로

1. 기본 경로 : `app폴더/static` 
2. 추가 경로 : `STATICFILES_DIRS`에 문자열 값으로 추가 경로 설정

## Static file 관련 settings 설정

### STATIC_URL

1. 설명 : 
    - 기본 경로 및 추가 경로에 위치한 정적 파일을 참조하기 위한 URL
    - 실제 파일이나 디렉토리 경로가 아니며, URL로만 존재
    - URL + STATIC_URL + 정적 파일 경로
2. 설정 :
    - `settings.py`에서 STATIC_URL 정의
        
        ```python
        # settings.py
        
        STATIC_URL = "static/"
        ```
        

### STATICFILES_DIRS

1. 정의 :
    - 정적 파일의 기본 경로 외에 추가적인 경로 목록을 정의하는 리스트
    - app 내의 static 디렉토리 경로를 사용하는 것(기본 경로) 외에 추가적인 정적 파일 경로 정의
2. 설정 :
    - 임의의 추가 경로 설정
    
    ```python
    # settings.py
    
    STATICFILES_DIRS = [
    	BASE_DIR / 'static'
    ]
    ```
    

## Static file 제공하기 - 절차

1. `settings.py`에 STATIC_URL (혹은 STATICFILES_DIRS 까지) 작성
2. 경로에 이미지 파일 배치
3. template에 DTL의 `static tag` 를 사용해 이미지 파일에 대한 경로 제공
    - `static tag`는 built-in tag가 아니기 때문에 `load tag` 를 사용해 import 후 사용 가능
    - 주의 : `base.html`에서 `load static`을 한다고 해서, `base.html`을 상속받은 자식템플릿에 `load static`이 적용되는 것이 아님
    
    ```html
    {% load static %}
    
    <img src="{% static 'articles/sample-1.png' %}" alt="img">
    ```
# Model

## Django Model

1. 설명 :
    - django는 model을 활용해서 DB를 처리
    - DB의 테이블을 정의하고 데이터를 조작할 수 있는 기능들을 제공 → 테이블 구조를 설계하는 청사진(blueprint)
    - 보통 DB에 데이터를 저장하고 조회하기 위해서는 SQL 쿼리문을 이용해야하나, django의 Model을 사용하면 이런 SQL 쿼리문의 도움 없이 데이터를 쉽게 처리 가능
        - django가 **ORM**을 제공하기 때문
2. 특징 : 
    - 클래스로 작성

## Model.py

1. model 클래스 작성
    
    ```python
    from django.db import models
    
    # Create your models here.
    class Article(models.Model): # models.Model은 상속 받을 상위 클래스
    		# title, content : table의 열 설계, Class의 인스턴스
        title = models.CharField(max_length=10)
        content = models.TextField()
    ```
    
    - 작성한 모델 클래스(위 예시에서 Article 클래스)는 최종적으로 DB에 테이블 구조를 만듦 → 테이블 설계는 python class를 사용
2. `models.Model`
    - 의미 : `django.db.models` 모듈의 `Model`이라는 부모 클래스를 상속받음
    - Model은 model과 관련된 모든 코드가 이미 작성 되어있는 클래스
    - 개발자는 가장 중요한 테이블 구조를 어떻게 설계할지에 대한 코드만 작성하도록 하기 위한 것 (상속을 활용한 프레임워크의 기능 제공)
3. `title = ~`, `content = ~` 
    - 클래스 변수명
    - 테이브의 각 필드(열) 이름

### Model Field

1. `Model Field` (`CharField` , `TextField` )
    - 데이터베이스 테이블의 열을 나타내는 중요한 구성 요소
    - 해당 필드에 저장되는 데이터 타입(Field types)과 제약 조건(Field options)을 정의
2. 필드 유형 Field Types : 데이터베이스에 저장될 데이터의 종류를 정의, models 모듈의 클래스로 정의되어 있음
    - 주요 필드 유형
        - 문자열 필드 : `CharField` , `TextField`
            - `CharField()` : 제한된 길이의 문자열을 저장, 필드의 최대 길이를 결정하는 `max_length`는 필수 옵션
            - `TextField()` : 길이 제한이 없는 대용량 텍스트를 저장, 무한대는 아니며 사용하는 시스템에 따라 달라짐
        - 숫자 필드 : `IntegerField`, `FloatField`
        - 날짜/시간 활용 : `DateField`, `TimeField`, `DateTimeField`
            - `DateTimeField` 의 필드 옵션
                - `auto_now` : 데이터가 저장될 때마다 자동으로 현재 날짜시간을 저장
                - `auto_now_add`: 데이터가 처음 생성될 때만 자동으로 현재 날짜 시간을 저장
        - 파일 관련 필드 : `FileField`, `ImageField`
3. 필드 옵션 Field Options : 필드의 동작과 제약 조건을 정의
    - 주요 필드 옵션
        - `null`: 데이터베이스에서 NULL 값을 허용할지 여부를 결정 (기본값: False)
        - `blank`: form에서 빈 값을 허용할지 여부를 결정 (기본값 : False)
        - `default`: 필드의 기본값을 성정
    - 제약 조건 Constraint : 특정 규칙을 강제하기 위해 테이블의 열이나 행에 적용되는 규칙이나 제한 사항

## Migrations

1. 설명 :
    - model 클래스의 변경사항(필드 생성, 수정 삭제 등)을 DB에 최종 반영하는 방법
    - `model.py` 작성
        - ≠ DB 변경
        - == 설계
2. Migrations 과정
    - model class(스케치) 만들기 →(makemigrations)→ migration 파일(최종 설계도) →migrate→ DB
    - Migrations 핵심 명령어
        
        ```bash
        python manage.py makemigrations
        ```
        
        - model class를 기반으로 최종 설계도(migration) 작성
        
        ```bash
        python manage.py migrate
        ```
        
        - 최종 설계도를 DB에 전달하여 반영
3. 추가 migrations
    - model class 변경사항(1)이 생겼다면, 반드시 새로운 설계도를 생성(2)하고, 이를 DB에 반영(3) 해야 함
    - (1) model class 변경
    - (2) makemigrations
        - 오류
            
            ```bash
            It is impossible to add the field 'created_at' with 'auto_now_add=True' to article without providing a default. This is because the database needs something to populate existing rows.
             1) Provide a one-off default now which will be set on all existing rows
             2) Quit and manually define a default value in models.py.
            ```
            
            - 추가하는 필드에 기본 값이 있어야 추가할 수 있다고 말하는 상황
                - 아래처럼 차있는 상황에서 새로 열을 빈 채로 추가하는 것은 논리적으로 오류가 있는 상황이기 때문
                
                | id | title | content |
                | --- | --- | --- |
                | 1 | 제목 1 | 내용 1 |
                | 2 | 제목 2 | 내용 2 |
            - 1번 : 현재 대화를 유지하면서 직접 기본 값을 입력하는 방법
            - 2번 : models.py에서 defalut 설정 후 다시 makemigration 시도
        - 새롭게 생성된 migration 파일
            - 새롭게 생성된 파일은 이전 파일에 대해 의존성이 존재함
            - 따라서 이전 파일을 지우면 X
                - 3번째 파일이 2번째 파일에 연관성이 있는 것은 아님
                - 번호는 단순히 최신 파일이라는 것을 의미할 뿐
            - 이처럼 Django는 설계도를 쌓아가면서 추후 문제가 생겼을 시 복구하거나 되돌릴 수 있도록 함
    - (3) migrate
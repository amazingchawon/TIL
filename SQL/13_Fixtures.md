# Fixtures

1. 설명 : 
    - Django가 데이터베이스로 가져오는 방법을 알고 있는 데이터 모음
    - 데이터는 데이터베이스 구조에 맞추어 작성 되어있음
    - Django에서는 fixtures를 사용해 앱에 초기 데이터(initial data)를 제공
2. 사용 목적 : 초기 데이터 제공
3. 필요성 :
    - 협업하는 유저 A, B
        - A가 먼저 프로젝트를 작업 후 원격 저장소에 push 진행
            - gitignore로 인해 DB는 업로드하지 않기 때문에 A가 생성한 데이터도 업로드 X
        - B가 원격 저장소에서 A가 push한 프로젝트를 pull or clone
            - 결과적으로 B는 DB가 없는 프로젝트를 받게 됨
    - 이처럼 프로젝트의 앱을 처음 설정할 때 동일하게 준비된 데이터로 데이터베이스를 미리 채우는 것이 필요한 순간이 있음
4. 사전 준비 :
    - M:N까지 모두 작성된 Django 프로젝트에서 유저, 게시글, 댓글 등 각 데이터를 최소 2-3개 이상 생성해두기
5. 주의 사항 : Fixtures 파일을 직접 만들지 말 것

## dumpdata

1. 설명 : 데이터베이스의 모든 데이터를 추출
2. 코드 :
    
    ```python
    python manage.py dumpdata [app_name[.ModelName] [app_name[.ModelName] ...]] > filename.json
    ```
    
3. 예시
    
    ```html
    $ python manage.py dumpdata --indent 4 articles.article > articles.json
    $ python manage.py dumpdata --indent 4 accounts.user > users.json
    $ python manage.py dumpdata --indent 4 articles.comment> comments.json
    ```
    
4. 모든 모델 한꺼번에 dump 하기 → 권장하지는 않음
    
    ```python
    # 3개의 모델을 하나의 json 파일로
    $ python manage.py dumpdata --indexnt 4 article
    ```
    

## loaddata

1. 설명 : Fixtures 데이터를 데이터베이스로 불러오기
2. Fixtures 파일 기본 경로 : `app_name/fixtures
    - Django는 설치된 모든 app의 디렉토리에서 fixtures 폴더 이후의 경로로 fixtures 파일을 찾아 load
3. 코드 :
    
    ```html
    $ python manage.py loaddata articles.json users.json comments.json
    ```
    
4. 주의사항 :
    - 만약 loaddata를 한번에 실행하지 않고 별도로 실행한다면 모델 관계에 따라 load 순서가 중요할 수 있음
        - comment는 article에 대한 key 및 user에 대한 key가 필요
        - artcle은 user에 대한 key가 필요
    - 즉, 현재 모델 관계에서는 user → article → comment 순으로 data를 load해야 오류가 발생하지 않음

### loaddata encoding 에러

1. dumpdata 시 추가 옵션 작성
    
    ```python
    $ python -Xutf8 manage.py dumpdata [생략]
    ```
    
2. 메모장 활용
    - 메모장으로 json 파일 열기
    - 다른 이름으로 저장
    - 인코딩을 UTF8로 선택 후 저장
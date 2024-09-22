# Admin site

## Automatic admin interface

1. 정의 :
    - Django가 추가 설치 및 설정 없이 자동으로 제공하는 관리자 인터페이스
    - 데이터 확인 및 테스트 등을 진행하는데 매우 유용

### 사용 절차

1. admin 계정 생성
    
    ```bash
    python manage.py createsuperuser
    ```
    
    - email은 선택사항이기 때문에 입력하지 않고 진행 가능
    - 비밀번호 입력 시 보안상 터미널에 출력되지 않으니 무시하고 입력 이어가기
2. DB에 생성된 admin 계정 확인
    - `DATABASE > Tables > auth_user` 에 생성됨
3. admin에 모델 클래스 등록
    
    ```python
    from . import models
    # 명시적 상대경로로 하면 아래와 같이 표현 가능
    # from .models import Article
    
    # admin site에 등록하다라고 암기!
    admin.site.register(Article)
    ```
    
    - `admin.py`에 작성한 모델 클래스를 등록해야만 admin site에서 확인 가능
4. admin site 로그인 후 등록된 모델 클래스 확인
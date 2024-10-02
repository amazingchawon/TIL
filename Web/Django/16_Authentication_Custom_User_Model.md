# Django Authentication System - 1

1. 정의 : 사용자 인증과 관련된 기능을 모아 놓은 시스템
2. 인증 Authentication 
    - 권한과 헷갈리지 말기
    - 사용자가 자신이 누구인지 확인하는 것, 신원 확인

# Custom User model

1. User Model
    - 여태까지 사용 했음
    - admin 이 만들어진다는거 자체가 User를 관리하는 Model이 있다는 뜻
    - 한번도 본 적이 없음
2. 기본 User Model의 한계
    - 우리는 지금까지 별도의 User 클래스 정의 없이 내장된 auth 앱에 작성된 User 클래스를 사용했음
        - `settings.py` 에서 `INSTALLED_APPS`에 `'django.contrib.auth'`라는 앱이 내부적으로 User 클래스를 가지고 있음
            - 이 user 클래스는 `AbstractUser`을 상속 받음
    - Django의 기본 User 모델은 username, password 등 제공되는 필드가 매우 제한적
    - 추가적인 사용자 정보(생년월일, 주소, 나이 등)가 필요하다면 이를 위해 기본 User Model을 변경하기 어려움
        - 별도의 설정 없이 사용할 수 있어 간편하지만, 개발자가 직접 수정하기 어려움
3. User Model 대체의 필요성
    - 프로젝트의 특정 요구사항에 맞춰 사용자 모델을 확장할 수 있음

## Custom User Model로 대체하기

1. AbstractUser 클래스를 상속받는 custom User 클래스 작성
    
    ```python
    from django.contrib.auth.models import AbstractUser
    from django.db import models
    
    # Create your models here.
    # 커스텀 User 모델로 대체하기 위한 class 작성
    class User(AbstractUser):
        pass
    ```
    
    - 기존 User 클래스도 Abstact User를 상속 받음 → 이 클래스만 상속받으면 custom User 클래스도 기존 User 클래스와 완전히 같은 모습을 가지게 됨
2. `settings.py` 변경 :
    
    ```python
    # settings.py
    
    AUTH_USER_MODEL = 'accounts.User'
    ```
    
    - Django 프로젝트에서 사용하는 기본 User 모델을 우리가 작성한 User 모델로 사용할 수 있도록 `AUTH_USER_MODEL` 값을 변경
    - `AUTH_USER_MODEL` : django 프로젝트의 User를 나타내는 데 사용하는 모델을 지정하는 속성
        - 프로젝트 중간에 `AUTH_USER_MODEL`을 **변경 할 수 없음**
            - 이미 프로젝트가 진행되고 있을 경우, 데이터베이스 초기화 후 진행
    - 기본값 : `auth.User` (앱.class 이름)
    - 변경값 : `accounts.User`
3. admin site에 대체한 User 모델 등록
    
    ```python
    # accounts/admin.py
    
    from django.contrib import admin
    from django.contrib.auth.admin import UserAdmin
    from .models import User
    
    # Register your models here.
    admin.site.register(User, UserAdmin)
    ```
    
    - 기본 User 모델이 아니기 때문에 등록하지 않으면 admin 페이지에 출력이 되지 않기 때문

## 프로젝트를 시작하며 반드시 User 모델을 대체해야 함

1. 이유 :
    - Django는 새 프로젝트를 시작하는 경우 비록 기본 User 모델이 충분하더라도 커스텀 User 모델을 설정하는 것을 강력하게 권장하고 있음
    - 커스텀 User 모델은 **기본 User 모델과 동일하게 작동**하면서도 **필요한 경우 나중에 맞춤 설정** 할 수 있기 때문
2. 주의 :
    - User Model 대체 작업은 프로젝트의 모든 migrations 혹은 첫 migrate을 실행하기 전에 작업을 마쳐야 함

### 초기화 방법

1. migrations 파일 지우기
2. 데이터 베이스 지우기
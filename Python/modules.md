# 모듈 Modules

한 파일로 묶인 변수와 함수의 모음, 특정한 기능을 하는 코드가 작성된 파이썬 파일(.py)

## 모듈 활용

### 모듈을 가져오는 방법

1. import문 사용 (권장)
    
    ```python
    import math
    
    print(math.sqrt(4)) # module 이름 명시 
    ```
    
2. from 절 사용
    
    ```python
    from math import sqrt
    
    print(sqrt(4)) # module 이름 명시 X
    ```
    

### 모듈 사용하기

1. `.` 연산자 (dot 연산자)
    
    “점의 왼쪽 객체에서 점의 오른쪽 이름을 찾아라”라는 의미
    
2. 주의사항
    - 서로 다른 모듈이 같은 이름의 함수를 제공할 경우 문제 발생
    - 마지막에 import된 함수로 대체됨
    - 따라서 모듈 내 모든 요소를 한번에 import 하는 `*` 표기는 권장하지 않음
3. 해결방안 :  `as` 키워드를 통해 별칭(alias)을 부여
    
    ```python
    from math import sqrt
    from my_math import sqrt as my_sqrt
    ```
    


# 파이썬 표준 라이브러리 Python Standard Library

파이썬 언어와 함께 제공되는 다양한 모듈과 패키지의 모음

## 패키지 Package

연관된 모듈들(.py)을 하나의 디렉토리에 모아 놓은 것

### 패키지 사용하기
```python
from my_package.math import my_math
from my_package.statistics import tools
```

1. PSL 내부 패키지 : 설치 없이 바로 import하여 사용
2. 외부 패키지 : pip를 사용하여 설치 후 import하여 사용

### pip

외부 패키지들을 설치하도록 도와주는 파이썬의 패키지 관리 시스템

1. 패키지 설치
    - 최신 버전, 특정 버전, 최소 버전을 명시하여 설치하라 수 있음
    
    ```bash
    pip install SomePackage
    pip install SomePackage == 1.0.5
    pip install SomePackage >= 1.0.4 
    ```
    
2. requests 외부 패키지 설치 및 사용 예시
    - global에 설치한 것
    - 개발할 때는 가상환경에 설치하는 게 일반적
    
    ```bash
    pip install requests
    ```
    
    ```python
    import requests
    
    url = 'https://random-data-api.com/api/v2/users'
    response = requests.get(url).json()
    
    print(response)
    ```
    

### 패키지 사용 목적

모듈들의 이름공간을 구분하여 충돌을 방지, 모듈들을 효율적으로 관리하고 재사용할 수 있도록 돕는 역할
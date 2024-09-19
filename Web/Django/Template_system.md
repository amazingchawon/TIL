# Template system

## Django Template system

1. 정의 : 데이터 표현을 제어하면서, 표현과 관련된 부분을 담당

## Django Template Language DTL

1. 정의 : Template에서 조건, 반복, 변수 등의 프로그래밍적 기능을 제공하는 시스템

### DTL Syntax

1. Variable
    
    ```html
    {{ variable }}
    {{ variable.attribute }}
    ```
    
    - `render` 함수의 세번째 인자로 딕셔너리 데이터를 사용
        - `render` 함수를 사용하여 `views.py`에서 정의한 변수를 template 파일로 넘겨 사용하는 것
    - 딕셔너리 key에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨
    - dot을 사용하여 변수 속성에 접근할 수 있음
2. Filter
    
    ```html
    {{ variable|filter }}
    {{ name|truncatewords:30 }}
    ```
    
    - 표시할 변수를 수정할 때 사용
        - `변수 | 필터`
    - chained(연결)이 가능하며 일부 필터는 인자를 받기도 함
    - 약 60개의 built-in template filters를 제공
3. Tags
    - 반복 또는 논리를 수행하여 제어 흐름을 만듦
    - 일부 태그는 시작과 종료 태그가 필요
        - `{% tag %}`
        - `{% if %} {% endif %}`
    - 약 24개의 built-in template tags를 제공
4. Comments
    - `{# #}` : 짧은 주석
    - `{% comment %} {% endcomment%}` : 긴 주석

## 템플릿 상속

1. 상속이 필요한 이유
    - 기본 템플릿 구조의 한계
        - 만약 모든 템플릿에 bootstrap을 적용하려면? 모든 파일을 수정해주어야할까? → 템플릿 상속으로 해결
2. 설명 : 
    - 페이지의 공통 요소를 포함하고, 하위 템플릿이 재정의 할 수 있는 공간을 정의하는 기본 skeleton 템플릿을 작성하여 상속 구조를 구축

### 상속 구조 만들기

1. skeleton
    
    ```html
    {% block 블락 이름 %}
    {% endblock 블락 이름 %} 
    ```
    
2. 하위 템플릿
    
    ```html
    {% extends '스켈리톤 html' %}
    {% block 블락 이름 %}
    -생략-
    {% endblock 블락 이름 %} 
    ```
    

### 상속 관련 DTL 태그

1. `extends` tag
    - 자식(하위) 템플릿이 부모 템플릿을 확장한다는 것을 알림
    - 반드시 자식 템플릿 최상단에 작성되어야 함 (2개 이상 사용 불가)
2. `block` tag
    - 하위 템플릿에서 재정의할 수 있는 블록을 정의
    - 상위 템플릿에 작성하면, 하위 템플릿이 작성할 수 있는 공간을 지정하는 것

## DTL 주의사항

1. Python처럼 일부 프로그래밍 구조(if, for 등)를 사용할 수 있지만 명칭을 그렇게 설계했을 뿐이지 Python 코드로 실행되는 것이 아니며 Python과는 관련 없음
2. 프로그래밍적 로직이 아니라 표현을 위한 것임을 명심하기
3. 프로그래밍적 로직은 되도록 view 함수에서 작성 및 처리할 것
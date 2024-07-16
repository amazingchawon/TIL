# Data Types
값의 종류와 그 값에 적용 가능한 연산과 동작을 결정하는 속성

## 목차
1. [Numeric Types](##Numeric-Types)
2. [Sequence Types](##Sequence-Types)
3. [Non-sequence Types](##-Non-sequence-Types)
4. [기타](##-기타)
## Numeric Types
### int 정수
1. 정의 : 정수를 표현하는 자료형
2. 진수 표현
    * 2진수 binary : 0b
    * 8진수 octal : 0o
    * 16진수 hexadecimal : 0x
### float 실수
1. 정의 : 실수를 표현하는 자료형, 실수에 대한 **근삿값**
2. 유한 정밀도 : 컴퓨터 메모리 용량이 한정돼있고 한 숫자에 대해 저장하는 용량이 제한됨
3. 실수 연산 시 주의사항
    * 부동소수점 에러 : 컴퓨터가 실수를 표현하는 방식으로 인해 발생하는 작은 오차
        * 원인 : 실수를 2진수로 변환하는 과정에서 발생하는 근사치 표현
        * 해결책 : Decimal 모듈 사용 (다양한 해결 방법 존재)
    ```python
    a = 3.2 - 3.1
    b = 1.2 - 1.1
    print(a) # 0.10000000009
    print(b) # 0.09999999987
    print(a == b) # False
    ```
4. 지수 표현 방식

    e 또는 E를 사용한 지수 표현
    ```python
    # 314 * 0.01
    number = 314e-2
    ```
## Sequence Types
여러 개의 값들을 **순서대로 나열하여**(정렬 X) 저장하는 자료형 (str, lsit, tuple, range)
1. 순서 Sequence
    
    값들이 순서대로 저장 (정렬 X)
2. 인덱싱 Indexing

    각 값에 고유한 인덱스(번호)를 가지고 있으며, 인덱스를 사용하여 특정 위치의 값을 선택하거나 수정할 수 있음
    * 인덱스 index : 시퀀스 내의 값들에 대한 고유한 번호로, 각 값의 위치를 식별하는데 사용되는 숫자 
3. 슬라이싱 Slicing

    인덱스 범위를 조절해 부분적인 값을 추출할 수 있음
4. 길이 Length

    len() 함수를 사용하여 저장된 값의 개수를 구할 수 있음
5. 반복 Iteration

    반복문을 사용하여 저장된 값들을 반복적으로 처리할 수 있음
### str 문자열
1. 정의 : 문자들의 순서가 있는 변경 불가능한 시퀀스 자료형
2. 문자열 표현 :
    * 작은따옴표(') 또는 큰따옴표(")로 감싸서 표현 (cf. PEP-8)
3. Escape sequence
    * backslash(\\) 뒤에 특정 문자가 와서 특수한 기능을 하는 문자 조합
    * 파이썬의 일반적인 문법 규칙을 잠시 탈출한다는 의미
    
        |예약 문자|내용|
        |---|---|
        | \\n |줄 바꿈|
        | \\t |탭|
        | \\\\ |백슬래시|
        | \\' |작은 따옴표|
        | \\" |큰 따옴표|
4. String Interpolation : 문자열 내에 변수나 표현식을 삽입하는 방법
    * f-string
        * 문자열에 f 또는 F 접두어를 붙이고 표현식을 {expression}으로 작성하는 문법
        * 문자열에 파이썬 표현식의 값을 삽입할 수 있음
            * 표현식
        * f-string에 dict 자료형의 값을 넣을 때에는 (\") 를 사용해서 indexing 해주어야 함
        ```python
        bugs = 'roaches'
        count = 13
        area = 'living room'

        # Debugging roached 13 living room
        print(f'Debugging {bugs} {counts} {area}')
        ```
5. str 특징
    * 인덱싱
    * 슬라이싱 : `str_name[staring_index:end index : step]`
        * step이 음수일 경우 : 시작 index가 끝 index보다 커야함
    * 길이
    * 불변
        ```python
        my_str = 'hello'

        # TypeError : 'str' object does not support item assignment
        my_str[1] = 'z'
        ```
    
### list
1. 정의 : 여러 개의 값을 순서대로 저장하는 변경 가능한 시퀀스 자료형
2. 리스트 표현 :
    * 0개 이상의 객체를 포함하며 데이터 목록을 저장
    * 대괄호([])로 표기
    * 데이터는 어떤 자료형도 저장할 수 있음
3. 특징 :
    * 인덱싱
    * 슬라이싱
    * 중첩된 리스트
    * 가변
### tuple
1. 정의 : 여러 개의 값을 순서대로 저장하는 변경 불가능한 시퀀스 자료형
2. 튜플 표현
    * 0개 이상의 객체를 포함하여 데이터 목록을 저장
    * 소괄호(())로 표기
    * 데이터는 어떤 자료형도 저장할 수 있음
    ```python
    # 빈 튜플
    my_tuple_1 = ()
    # 값이 하나인 튜플 쉼표(,) 필수!
    # 쉽표가 없다면 int로 인식함
    my_tuple_2 = (1,)
    ```
3. 특징
    * 인덱싱
    * 슬라이싱
    * 길이
    * 불변
4. 사용
    * (튜플의 불변 특성을 사용) 안전하게 여러 개의 값을 전달, 그룹화, 다중 할당 등
    * 개발자가 직접 사용한다기 보다는 '**파이썬 내부 동작**'에서 주로 사용됨
    ```python
    # 다중 할당 (괄호가 없어도 가능 / x, y에 재할당 가능)
    x, y = (10, 20)
    print(x) # 10
    print(y) # 20
    ```
### range
1. 정의 : 연속된 정수 시퀀스를 **생성**하는 변경 불가능한 자료형
2. range 표현
    * range(시작 값, 끝 값, 증가 값)
    * range(n) : 0부터 n-1까지의 숫자 시퀀스
        * 끝 값이 빠지는 이유 : 시작이 0부터여서 n개를 만들려면 n-1까지만 해야함
    * range(n, m) : n부터 m-1까지의 숫자 시퀀스
3. 특징
    * 증가 값 생략 시 , 1씩 증가
    * 증가 값이 음수이면 감소 / 양수이면 증가
    * 증가 값이 0이면 에러
    * 증가 값이 음수이면 **시작 값이** 끝 값보다 **커야 함**
    * 증가 값이 양수이면 **시작 값이** 끝 값보다 **작아야 함**
4. 사용
    * 주로 반복문에 사용
    ```python
    for i in range(5) :
        print(i)
    ```

## Non-sequence Types
### dict
1. 정의 : key - value 쌍으로 이루어진 순서와 중복이 없는 변경 가능한 자료형
2. 딕셔너리 표현
    * key : **변경 불가능한 자료형**만 사용 가능 (str, int, float, tuple, range … ), 하나의 dict에서 중복 불가
        * int는 왜 변경 불가한가? numeric type은 불변
            
            ```python
            a = 100
            # 재할당
            a = 99 
            ```
            
    * value : 모든 자료형 사용 가능
    * 중괄호로 표기
3. 딕셔너리 특징
    * 가변성
        * 추가
        * 변경
    * key에 접근해 value를 얻을 수 있음
        
        ```python
        my_dict = {'apple', 12, 'list' : [1, 2, 3]}
        
        # 딕셔너리는 키에 접근해 값을 얻어냄
        print(my_dict['apple']) # 12
        
        # 추가
        my_dict = ['banana'] = 50
        print(my_dict) # {'apple', 12, 'list' : [1, 2, 3], 'banana' : 50}
        
        # 변경
        my_dict = ['banana'] = 100
        print(my_dict) # {'apple', 12, 'list' : [1, 2, 3], 'banana' : 100}
        ```
        
4. 딕셔너리의 사용
    * 리스트 사용 시
        * [국가 1, 국가 2, … 국가 n]
        * 한국을 찾으세요 → 처음부터 탐색할 수 밖에 없음
    * 딕셔너리 사용 시 → 바로 찾기 가능
    * API → 딕셔너리 구조
### set
1. 정의 : 순서와 중복이 없는 **변경 가능**한 자료형
2. set 표현
    * 수학에서의 집합과 동일한 연산 처리 가능
    * 중괄호로 표시
        * dict과의 혼동 가능성
            * `a = {}` : dict
    
    ```python
    my_set_1 = set()
    my_set_2 = {1, 2, 3}
    my_set_3 = {1, 1, 1}
    
    print(my_set_1)  # set()
    print(my_set_2)  # {1, 2, 3}
    print(my_set_3)  # {1}
    ```
    
3. set의 집합 연산
    
    ```python
    my_set_1 = {1, 2, 3}
    my_set_2 = {3, 6, 9}
    
    # 합집합
    print(my_set_1 | my_set_2) # {1, 2, 3, 6, 9}
    # 차집합
    print(my_set_1 - my_set_2) # {1, 2}
    # 교집합
    print(my_set_1 & my_set_2} # {3}
    ```
    
4. set의 사용
    * 중복 제거
    * 집합 연산

## 기타
### None
1. 정의 : 파이썬에서 ‘값이 없음’을 표현하는 자료형
### Boolean
1. 정의 : 참(True)과 거짓(False)을 표현하는 자료형
2. boolean 표현
    - 비교 / 논리 연산의 평가 결과로 사용됨
    - 주로 조건 / 반복문과 함께 사용

## Collection
여러 개의 항목 또는 요소를 담는 자료 구조 (str, list, tuple, set, dict)
|컬렉션|변경 가능 여부|순서 여부|
|:---:|:---:|:---:|
| str |X|O|
| list |O|O|
| tuple |X|O|
| dict |O|X|
| set |O|X|
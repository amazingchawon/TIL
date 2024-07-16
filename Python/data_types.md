# Data Types
값의 종류와 그 값에 적용 가능한 연산과 동작을 결정하는 속성
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
1. 정의 : 문자들의 순서가 있는 변경 불가능한 시쿼스 자료형
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
### tuple
### range

## Non-sequence Types
### set
### dict

## 기타
### Boolean
### None
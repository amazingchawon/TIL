# 제어문 Control Statement

코드의 실행 흐름을 제어하는데 사용되는 구문 (코드는 원래 위에서 아래 방향으로 실행)

**조건** 에 따라 코드 블록을 실행하거나 **반복** 적으로 코드 실행

# 조건문 Conditional Statement

주어진 조건식을 평가하여 해당 조건이 참(True)인 경우에만 코드 블록을 실행 (if / elif  / else)

## if statement

1. if statment의 기본 구조
    
    ```python
    if 표현식 :
    	코드 블록
    elif 표현식 :
    	코드 블록
    else : 
    	코드 블록
    ```
    
    - else, elif 는 단독으로 존재할 수 없음
2. 복수 조건문 :  조건식을 동시에 검사하는 것이 아니라 **순차적**으로 비교
3. 중첩 조건문

# 반복문 Loop Statement

주어진 코드 블록을 여러 번 반복해서 실행하는 구문

## for statement

1. 정의 : 특정 작업을 반복적으로 수행 (종료 범위 지정), 임의의 시퀀스의 항목들을 그 시퀀스에 들어있는 순서대로 반복
2. 기본 구조
    
    ```python
    for 변수 in 반복 가능한 객체 :
    	코드 블록
    ```
    
3. 반복 가능한 객체 Iterable
    - 반복문에서 순회할 수 있는 객체
    - Sequence 객체 뿐만 아니라 dict, set 등도 포함
4. 반복문 변수명 설정
    
    ```python
    items = ['apple', 'banana', 'coconut']
    
    for item in items :
    	print(item)
    ```
    
5. 딕셔너리 순회
    
    ```python
    my_dict = {'x' : 10, 'y' : 20, 'z' : 30}
    
    for key in my_dict :
    	print(key) # key 값이 출력됨
    ```
    
6. 중첩된 반복문
    
    ```python
    outers = ['A', 'B']
    inners = ['c', 'd']
    
    for outer in outers :
    	for inner in inners :
    		print(outer, inner)
    '''
    Ac
    Ad
    Bc
    Bd
    '''
    ```
    

## while statement

1. 정의 : 주어진 조건이 참(True)인 동안 반복해서 실행 == 조건식이 거짓(False)가 될 때까지 반복
2. 기본 구조
    
    ```python
    while 조건식 :
    	코드 블록
    ```
    
3. 활용
    - 반복 횟수가 불명확하거나 조건에 따라 반복을 종료해야 할 때 유용
    - 예를 들어 사용자의 입력을 받아서 특정 조건이 충족될 때까지 반복하는 경우

## 반복 제어

for문과 while은 매 반복마다 본문 내 모든 코드를 실행하지만 때때로 일부만 실행하는 것이 필요할 때가 있음

### break

반복을 즉시 중지

### continue

다음 반복으로 건너뜀

### pass

아무런 동작도 수행하지 않고 넘어감

## List Comprehension

1. 정의 : 간결하고 효율적인 리스트 생성방법 *남발하지 말자, for문을 줄이려는 것이 아닌, 리스트 생성이 목적!
2. 구조
    
    ```python
    [expression for 변수 in interable]
    
    list(expression for 변수 if iterable)
    ```
    
3. 활용 예시
    - 2차원 배열 생성시 (인접행렬 생성시)
    
    ```python
    data1 = [[0] * 10 for _ in range(5)]
    data2= [[0 for _ in range(5)] for _ in range(5)]
    ```
    

# 참고

## 모듈 내부 살펴보기

내장 함수 help를 사용해 모듈에 무엇이 들어있는지 확인 가능

## enumerate

1. `enumerate(iterable, start=0)` 
    - 설명 : iterable 객체의 각 요소에 대해 인덱스와 함께 반환하는 내장함수
    - start : 인덱스 값 시작번호
2. 예시
    ```python
    fruits = ['apple', 'banana', 'cherry']

    for index, fruit in enumerate(fruits) :
        print(f'인덱스 {index} : {fruit}')
    ```
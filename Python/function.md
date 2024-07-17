# 함수 Functions


특정 작업을 수행하기 위한 재사용 가능한 코드 묶음

## 함수구조

```python
def make_sum(pram1, pram2) :
'''이 것은 두수를 받아 두 수의 합을 반환하는 함수입니다.
>>> make_sum(1, 2)
'''
return pram1 + pram2
```

### 함수의 정의와 호출

1. 정의
    - `def function_name(parameter) :`
    - 매개변수(parameter)는 함수에 전달되는 값을 나타냄
2. Docstring
    - 함수 body 앞에 **선택적**으로 작성 가능한 함수 설명서
3. 함수 Body
    - 콜론(:) 다음에 들여쓰기 된 코드 블록
    - 함수가 실행 될 때 수행되는 코드를 정의
4. 함수 반환 값
    - 함수는 필요한 경우 결과를 반환할 수 있음
    - return 키워드 이후 반환할 값을 명시
    - return 문은 함수의 실행을 종료하고, 결과를 호출 부분으로 반환 (return 이후는 실행되지 않음)
    - cf ) return을 작성하지 않았을 경우 : python이 자동으로 return None 붙여줌
5. 함수 호출 Function call
    - 함수를 사용하기 위해서는 호출이 필요
    - `function_name(argument)` 를 통해 호출
        - argument(인자)가 없을 수도
    - 호출 부분에 전달된 인자(argument)는 함수 정의 시 작성한 매개변수(parameter)에 대입됨
6. 참고 : print와 return
    
    ```python
    def make_sum(pram1, pram2) :
    '''이 것은 두수를 받아 두 수의 합을 반환하는 함수입니다.
    >>> make_sum(1, 2)
    '''
    return pram1 + pram2
    
    result = make_sum(100, 30)
    return_value = print(result) # 130 출력됨
    print(return_value) # None : print는 return 값이 없는 함수, return과 print는 다르다
    ```
    

## 매개변수와 인자

### 매개변수 parameter

함수를 **정의**할 때, 함수가 받을 값을 나타내는 변수

### 인자 argument

함수를 **호출**할 때, 실제로 전달되는 값

### 다양한 인자 종류

1. 위치 인자 Positional Arguments
    - 함수 호출 시 인자의 **위치에 따라** 전달되는 인자
    - 위치인자는 함수 호출 시 **반드시 값을 전달**해야 함
    
    ```python
    def greet(name, age) :
    	print(f'안녕하세요, {name}님! {age}살이시군요.')
    
    greet('Alice', 25) # 안녕하세요, Alice님! 25살이시군요.
    greet('amazingchawon') # TypeError : greet() missing 1 required positional argument : 'age'
    ```
    
2. 기본 인자 값 Default Arguments
    - 함수 정의에서 매개변수에 **기본 값**을 할당하는 것
    - 함수 호출 시 인자를 **전달하지 않으면**, 기본 값이 매개변수에 할당됨
    
    ```python
    def greet(name, age = 30) :
    	print(f'안녕하세요, {name}님! {age}살이시군요.')
    
    greet('Bob') # 안녕하세요, Bob님! 30살이시군요.
    ```
    
3. 키워드 인자 Keyword Arguments
    - 함수 호출 시 **인자의 이름과 함께 값**을 전달하는 인자
    - 매개변수와 인자를 일치시키지 않고, 특정 매개변수에 값을 할당할 수 있음
    - **인자의 순서는 중요하지 않으며**, 인자의 이름을 명시하여 전달
    - 단, 호출 시 Keyword 인자는 Positional 인자 뒤에 위치해야 함
    
    ```python
    def greet(name, age) :
    	pring(f'안녕하세요, {name}님! {age}살이시군요.')
    
     greet(age=35, name='Dave') # Keyword의 순서는 상관 없음 
     greet(age=35, 'Dave') # positional argument follows keyword argument
    ```
    
4. 임의의 인자 목록 Arbitrary Arguments Lists
    - 정해지지 않은 개수의 인자를 처리하는 인자
    - 함수 정의 시 매개변수 앞에 `*`를 붙여 사용 (보통 args라고 씀)
    - 여러 개의 인자를 tuple로 처리
    - EX ) print 함수도 같은 원리
    
    ```python
    define calculate_sum(parm, *args) :
    	print(args)
    	total = sum(args) - parm
    	print(f'합계 : {total}')
    	
    calculate_sum(1, 2, 3)
    '''
    (2, 3)
    합계 : 4
    '''
    ```
    
5. 임의의 키워드 인자 목록 Arbitrary Keyword Arguments Lists
    - 정해지지 않은 개수의 keyword 인자를 처리하는 인자
    - 함수 정의 시 매개변수 앞에 `**` 를 붙여 사용 (보통 kwargs 라고 씀)
    - 여러 개의 인자를 **dictionary**로 묶어 처리
        - cf ) `for keys, values in kwarg.items() :` 등으로 활용 가능
    
    ```python
    def print_info(**kwargs) :
    	print(kwargs)
    
    print_info(name='Eve', age=30)
    ```
    

### 함수 인자 권장 작성순서

1. 위치 → 기본 → 가변 → 가변 키워드 (positional → default → arbitrary → arbitrary keyword) 
2. 단, 모든 상황에 적용되는 절대적인 규칙은 아니며, 상황에 따라 유연하게 조정될 수 있음
    
    ```python
    def func(pos1, pos2, desfault_arg = 'default', *args, **kwargs) :
    	print('위치 1 : ', pos1)
    	print('위치 2 : ', pos2)
    	print('기본 : ', default_arg)
    	print('임의의 인자 목록 : ', args)
    	print('임의의 키워드 인자 목록 : ', kwargs)
    
    func(1, 2, 3, 4, 5, 6, key1 = 'value1', key2 = 'value2')
    
    '''
    위치 1 : 1
    위치 2 : 2
    기본 : 3
    임의의 인자 목록 : (4, 5, 6)
    임의의 키워드 인자 목록 : {'key1' : 'value1','key2' : 'value2' }
    '''
    ```
    

## 재귀 함수

1. 정의 : 함수 내부에서 자기 자신을 호출하는 함수
2. 사용 :  재귀 호출의 결과를 이용하여 문제를 작은 단위의 문제로 분할하고, **분할된 문제들의 결과를 조합**하여 최종 결과를 도출
3. 특징
    - 특정 알고리즘 식을 표현할 때 변수의 사용이 줄어들며, 코드의 가독성이 높아짐
    - 1개 이상의 base case(종료되는 상황)가 존재하고, 수렴하도록 작성
4. 재귀 함수를 사용하는 이유
    - 문제의 자연스러운 표현
        - 복잡한 문제를 간결하고 직관적으로 표현 가능
    - 코드 간결성
        - 상황에 따라 반복문보다 알고리즘 코드가 더 간결하고 명확해질 수 있음
    - 수학적 문제 해결
        - 수학적 정의가 재귀적으로 표현되는 경우, 직접적인 구현 가능
5. **중요**
    - 종료 조건을 명확히
    - 반복되는 호출이 종료 조건을 향하도록
6. EX) 팩토리얼
    
    ```python
    def factorial (n) :
    # 종료 조건 : n이 0이면 1을 반환
    if n == 0 :
    	return 1
    else : 
    	# 재귀 호출 : n과 n-1의 팩토리얼을 곱한 결과를 반환
    	return n * factorial(n-1)
    ```
    

## 내장 함수 Built-in funtion

파이썬이 기본적으로 제공하는 함수, 별도의 import 없이 바로 사용 가능

[공식 문서 : built-in function](https://docs.python.org/3/library/functions.html)

### 자주 사용하는 내장 함수

1. `print()` 
2. `len()` 
3. `max(), min()` 
4. `sum()` 
5. `sorted(iterable)` 
    - `sorted(iterable, reverse=True)`

### 유용한 내장 함수 map & zip

1. `map(function, iterable)` 
    - 정의 : 순회 가능한 데이터구조(iterable)의 모든 요소에 함수를 적용하고, 그 결과를 map object로 반환
    - iterable : 반복 가능한 객체(Collection)
    
    ```python
    numbers = [1, 2, 3]
    result = map(str, numbers)
    
    print(result) # <map object at 0x0000239C915D760>
    print(list(result)) # ['1', '2', '3']
    ```
    
2. `zip(*iterables)` 
    - 임의의 iterable을 모아 tuple을 원소로 하는 zip object를 반환
    
    ```python
    girls = ['jane', 'ashley']
    boys = ['peter', 'jay']
    pair = zip(girls, boys)
    
    print(pair) # <zip object at 0x00001C76DE58700>
    print(list(pair)) # [('jane', 'peter'), ('ashley', 'jay)]
    ```
    
    - 활용 : 2차원 리스트 같은 column(열) 요소를 동시에 조회할 때

## 함수와 Scope

함수는 코드 내부에 **local scope(지역)** 를 생성하며, 그 외의 공간인 **global scope(전역)** 로 구분

### 범위와 변수 관계

1. scope
    - global scope : 코드 어디에서든 참조할 수 있는 공간
    - local scope : 함수가 만든 scope (함수 내부에서만 참조 가능)
2. variable
    - global variable : global scope에 정의된 변수
    - local variable : local scope에 정의된 변수

### 변수 수명주기 lifecycle

변수의 수명주기는 변수가 선언되는 위치와 scope에 따라 결정됨

1. built-in scope
    
    파이썬이 실행된 이후부터 영원히 유지
    
2. global scope
    
    모듈이 호출된 시점 이후, 인터프리터가 끝날 때까지 유지
    
    **when the module is imported until the end of the application** - unless the variable is explicitly deleted.
    
3. local scope
    
    함수가 호출될 때 생성되고, 함수가 종료될 때까지 유지
    

### 이름 검색 규칙 Name Resolution

- 파이썬에서 사용되는 이름(식별자)들은 특정한 이름공간(namespace)에 저장되어 있음
- 아래와 같은 순서로 이름을 찾아 나가며, LEGB Rule이라고 부름
    - Local scope : 지역 범위, 현재 작업 중인 범위
    - Enclosed scope : 지역 범위 한 단계 위 범위
        - 중첩된 함수를 위해 존재하는 범위
        - local scope : nested function , enclosed scope : enclosing function
    - Global scope : 최상단에 위치한 범위
    - Built-in scope : 모든 것을 담고 있는 범위, 정의하지 않고 사용할 수 있는 모든 것)
- 함수 내에서는 바깥 scope의 변수에 접근 가능하나 수정할 수 없음

```python
a = 1
b = 2

def enclosed() :
	a = 10
	c = 3
	
	def local(c) :
		# c는 parameter일 뿐, 아래 500(argument)이 출력됨
		print(a, b, c) # 10 2 500
		
	local(500)
	print(a, b, c) # 10 2 3
	
enclosed()
print(a,b) # 1 2
```

### global 키워드

1. 사용 
    - 변수의 scope를 전역 범위로 지정하기 위해 사용
    - 일반적으로 함수 내에서 전역 변수를 수정하려는 경우에 사용
2. 주의 
    - global 키워드 선언 전에 참조 불가
    
    ```python
    num = 0 
    
    def increment() :
    	# SyntaxError : name 'num' is used prior to global declaration
    	print(num)
    	global num
    	num += 1
    ```
    
    - 매개변수에는 global 키워드 사용 불가
    
    ```python
    num = 0
    
    def increment(num) :
        #SyntaxError: name 'num' is parameter and global
        global num
        num += 1
    ```
    

## Packing & Unpacking

### Packing

1. 정의 : 여러 개의 값을 하나의 변수에 묶어서 담는 것
2. 예시
    - 변수에 담긴 값들은 **tuple** 형태로 묶임
    
    ```python
    packed_values = 1, 2, 3, 4, 5
    print(packed_values) # (1, 2, 3, 4, 5)
    ```
    
3. `*` 을 활용한 패킹
    - **list**로 패킹
    
    ```python
    numbers = [1, 2, 3, 4, 5]
    a, *b, c = numbers
    
    print(a) # 1
    print(b) # [2, 3, 4]
    print(c) # 5
    ```
    

### Unpacking

1. 정의  : 패킹된 변수의 값을 개별적인 변수로 분리하여 할당하는 것
2. 예시
    - tuple이나 list 등의 객체의 요소들을 개별 변수에 할당
    
    ```python
    packed_values = 1, 2, 3, 4, 5
    a, b, c, d, e = packed_values
    
    print(a, b, c, d, e) # 1 2 3 4 5
    
    # ValueError: not enough values to unpack
    a, b, c, d, e, f = packed_values
    ```
    
3. `*` 을 사용한 unpacking
    - list의 요소를 unpacking하여 인자로 전달
    
    ```python
    def my_funtion(x, y, z) : 
    names = ['alice', 'jane', 'peter']
    my_function(*names) # alice jane peter
    
    # TypeError: my_function() missing 1 required positional argument: 'z'
    names = ['alice', 'jane']
    my_function(*names)
    ```
    
4. `**` 을 활용한 unpacking
    - dictionary의 key-value 쌍을 unpacking하여 함수의 keyword 인자로 전달
    - 주의 :  keyword 인자라서 이름을 맞춰 주어야 함!!
    
    ```python
    def my_funtion(x, y, z) :
    	print(x, y, z)
    
    my_dict = {'x' : 1, 'y' : 2, 'z' : 3}
    my_fuction(**my_dict) # 1 2 3
    ```
    


## 참고

### Lambda expressions 람다 표현식

1. 정의 : **익명 함수**를 만드는 데 사용되는 표현식, 한 줄로 간단한 함수를 정의
2. 표현식 구조 : `lamda 매개변수1, 매개변수2 : 표현식` 
    - `lamda` 키워드 : lambda 함수를 선언하기 위해 사용되는 키워드
    - 매개변수 : 여러 개의 매개변수가 있을 경우 `,` 로 구분
    - 표현식 : 함수의 실행되는 코드 블록으로, 결과값을 반환하는 표현식으로 작성
3. 특징
    - 간단한 연산이나 함수를 한 줄로 표현할 때 사용
    - 함수를 매개변수로 전달하는 경우에도 유용하게 활용 → 1회용으로 쓸 때 유용
    - map 함수하고 활용
    ```python
    numbers = [1, 2, 3, 4, 5]

    def square(x) :
        return x**2

        # lambda 미사용
        squared1 = list(map(square, numbers))
        print(squared1) # [1, 4, 9, 16, 25]

        # lambda 사용
        squared2 = list(map(lambda x : x**2, numbers))
        print(squared2) # [1, 4, 9, 16, 25]
    ```
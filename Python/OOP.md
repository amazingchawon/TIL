# 객체 지향 프로그래밍

## 객체 지향 프로그래밍의 등장 배경

### 소프트웨어 위기 Software Crisis

하드웨어의 발전으로 컴퓨터 계산용량과 문제의 복잡성이 급격히 증가함에 따라 소프트웨어에 발생한 충격 → 절자 지향 프로그래밍의 한계

### 절차 지향 프로그래밍 Procedural Programming

1. 정의 : 프로그램을 ‘데이터’와 ‘절차’로 구성하는 방식의 프로그래밍 **패러다임**
2. 특징 :
    - ‘데이터’와 해당 데이터를 처리하는 ‘함수(절차)’가 분리되어 있으며, 함수 호출의 흐름이 중요
    - 코드의 순차적인 흐름과 함수 호출에 의해 프로그램이 진행
    - 데이터를 재사용하기보다는 처음부터 끝까지 실행되는 결과물이 중요한 방식 (재사용하기 어렵다)

## 객체 지향 프로그래밍 Object Oriented Programming

1. 정의 : 데이터와 해당 데이터를 조작하는 메서드를 하나의 객체(클래스)로 묶어 관리하는 방식의 프로그래밍 **패러다임**
2. 절차 지향과의 차이
    - 절차 지향 : 함수 호출의 흐름이 중요
    - 객체 지향 : 객체 간의 상호작용과 메서드 전달이 중요
3. 주의 : 절차 지향과 객체 지향은 대조되는 개념이 아니다!
    - 객제 지향은 기존 절치 지향을 기반으로 두고 보완하기 위해 객체라는 개념을 도입해 상속, 코드의 재사용성, 유지보수성 등의 이점을 가지는 패러다임

# 객체

### 클래스 Class

- 파이썬에서 타입을 표현하는 방법
- 객체를 생성하기 위한 설계도
- 데이터와 기능을 함께 묶는 방법을 제공

### 객체 Object

1. 정의 : 클래스에서 정의한 것을 토대로 메모리에 할당된 것, **속성(변수)** 과 **행동(메서드)** 으로 구성된 모든 것
2. 타입(type) : 어떤 연산자(operator)와 조작(method)이 가능한가?
3. 속성(attribute) : 어떤 상태(데이터)를 가지는가?
4. 조작법(method) : 어떤 행위(함수)를 할 수 있는가?

### 인스턴스 Instance

1. 정의 : 클래스의 속성과 행동을 기반으로 생성된 개별 객체, 클래스로 만든 객체를 인스턴스라고 부름
2. (EX) : 아이유는 객체다 (O), 아이유는 인스턴스다(X), 아이유는 가수의 인스턴스다(O)
    - 하나의 객체(object)는 특정 타입의 인스턴스(instance) 다.

### 예시

```python
name = 'Alice'
print(type(name)) # <class 'str'>
```

- 변수 name의 타입 : str 클래스
- 변수 name : str 클래스의 인스턴스

# 클래스 Class

## 클래스 정의

`class ClassName`

1. `class` : 클래스 키워드
2. `ClassName` : 클래스 이름은 파스칼 케이스 방식으로 작성

## 클래스 구성요소

```python
# 클래스 정의
class Person:
    blood_color = 'red'

    def __init__(self, name) :
        self.name = name
    
    def singing(self) :
        return f'{self.name}이 노래합니다.'

# 인스턴스 생성
singer1 = Person('iu')

# (인스턴스) 메서드 호출
print(singer1.singing()) # iu이 노래합니다.

# 속성(변수) 접근
print(singer1.blood_color) # red

# 인스턴스 속성(변수)
print(singer1.name) # iu
```

### 생성자 메서드 `def __init(self, name) :`

1. 정의 : 객체를 생성할 때 자동으로 호출되는 특별한 메서드 (Magic method)
2. 사용 :
    - `__init__` 이라는 이름의 메서드로 정의
    - 객체 초기화 담당
    - 생성자 함수를 통해 인스턴스를 생성하고 필요한 초기값을 설정

### 인스턴스 변수 `self.name = name`

1. 정의 : 인스턴스(객체)마다 별도로 유지되는 변수
2. 특징 : 인스턴스마다 독립적인 값을 가지며, 인스턴스가 생성될 때마다 초기화됨

### 클래스 변수 `blood_color = 'red'`

1. 정의 : 클래스 내부에 선언된 변수
2. 특징 : 클래스로 생성된 모든 인스턴스들이 공유하는 변수

### 인스턴스 메서드 `def singing(self) :`

1. 정의 : 각각의 인스턴스가 호출할 수 있는 메서드
2. 특징 : 인스턴스 변수에 접근하고 수정하는 등의 작업을 수행

# 메서드

## 인스턴스 메서드 Instance Method

### 인스턴스 메서드 구조

1. 정의 : 클래스로부터 생성된 각 인스턴스에서 호출할 수 있는 메서드
2. 역할 : 인스턴스의 상태를 조작하거나 동작을 수행
3.  규칙 : 반드시 첫 번째 매개변수로 인스턴스 자신(self)을 전달받음
    
    `def instance_method(self, arg1, …)` 
    

### self 동작 원리

1. (EX)
    - upper 메서드를 사용해 문자열 ‘hello’를 대문자로 변경하기
        - 객체 지향 방식의 메서드로 호출하는 표현 (단축형 호출)
        
        ```python
        'hello'.upper()
        ```
        
    - 하지만 실제 파이썬 내부 동작은 다음과 같이 진행됨
        - 함수 중심 (절차 지향)
        
        ```python
        str.upper('hello')
        ```
        

    → str 클래스가 upper 메서드를 호출했고, 그 첫번째 인자로 문자열 인스턴스(hello)가 들어간 것

## 클래스 메서드 Class Method

클래스가 호출하는 메서드 → 클래스 변수를 조작하거나 클래스 레벨의 동작을 수행

### 클래스 메서드 구조

```python
@classmethod
def class_method(cls, args, …) :
	pass
```

1. `@classmethod` : 데코레이터를 사용하여 정의
2. `cls` : 호출 시, 첫번째 인자로 해당 메서드를 호출하는 클래스가 전달됨, 전달될 때 사용하는 매개변수 이름 = `cls` 

## 스태틱 메서드 Static Method

클래스와 인스턴스와 상관없이 독립적으로 동작하는 매서드, 주로 클래스와 관련이 있지만 인스턴스와 상호작용이 필요하지 않은 경우에 사용

### 스테틱 메서드 구조

```python
Class MyClass :
	 @staticmethod
	 def static_method(arg1, ...) :
		 pass
```

1. `@staticmethod` : 데코레이터를 사용하여 정의
2. 호출 시 필수적으로 작성하여 할 매개변수가 없음
    
    → 객체 상태나 클래스 상태를 수정할 수 없음, 단지 기능(행동)만을 위한 매서드로 사용
    
3. 특징 : 호출은 클래스가!

## 인스턴스와 클래스 간의 이름 공간

독립적인 이름공간을 가진다

### 독립적인 이름공간을 가지는 이점

- 각 인스턴스는 독립적인 메모리 공간을 가지며, 클래스와 다른 인스턴스 간에는 서로의 데이터나 상태에 직접적인 접근이 불가능
- 객체 지향 프로그래밍의 중요한 특성 중 하나로, 클랙스와 인스턴스를 모듈화하고 각각 개체가 독립적으로 동작하도록 보장
- 이를 통해 클래스와 인스턴스는 다른 객체들과의 상호작용에서 서로 충돌이나 영향을 주지 않으면서 독립적으로 동작할 수 있음

→ 코드의 가독성, 유지보수성, 재사용성을 높이는데 도움을 줌

## 매직 메서드

1. 종류 : 인스턴스 메서드
2. 역할 : 특정 상황에 자동으로 호출되는 메서드
3. 구조 : Double underscore(__)가 있는 메서드는 특수한 동작을 위해 만들어진 메서드

## 데코레이터

다른 함수의 코드를 유지한 채로 수정하거나 확장하기 위해 사용되는 함수, 원래 함수의 중간에 끼어드는 구문은 불가!
```python
# 데코레이터 정의

def my_decorator(func) :
	def wrapper() :
	# 함수 실행 전에 수행할 작업
	print('함수 실행 전')
	# 원본 함수 호출
	result = func()
	# 함수 실행 후에 수행할 작업
	print('함수 실행 후')
	return result
return wrapper
```

```python
# 데코레이터 사용
@my_decorator
def my_function() :
	print('원본 함수 실행')

my_function()
'''
함수 실행 전
원본 함수 실행
함수 실행 후
'''
```

# 상속 Inheritangce

기존 클래스의 속성과 메서드를 물려받아 새로운 하위 클래스를 생성하는 것

## 상속이 필요한 이유

1. 코드 재사용
    - 상속을 통해 기존 클래스의 속성과 메서드를 재사용할 수 있음
    - 새로운 클래스를 작성할 때 기존 클래스의 기능을 그대로 활용할 수 있으며, 중복된 코드를 줄일 수 있음
2. 계층 구조
    - 상속을 통해 클래스들 간의 계층 구조를 형성할 수 있음
    - 부모 클래스와 자식 클래스 간의 관계를 표현하고, 더 구체적인 클래스를 만들 수 있음
3. 유지 보수의 용이성
    - 상송을 통해 기존 클래스의 수정이 필요한 경우, 해당 클래스만 수정하면 되므로 유지 보수가 용이해짐
    - 코드의 일관성을 유지하고, 수정이 필요한 범위를 최소화 할 수 있음

## 클래스 상속

```python
class 부모 :
...

class 자식(부모) :
...
```

## 다중 상속

1. 정의 : 둘 이상의 상위 클래스로부터 여러 행동이나 특징을 상속받을 수 있는 것
2. 특징 :
    - 상속받는 모든 클래스의 요소를 활용 가능함
    - (상속받는 클래스들 사이에) 중복된 속성이나 메서드가 있는 경우 **상속 순서** 에 의해 결정됨
    
    ```python
    class 부모 1 :
    ...
    
    class 부모 2 :
    ...
    class 자식(부모2, 부모1) :
    ...
    # 자식은 부모 2의 속성이나 메서드를 상속함
    ```
    

### 다이아몬드 문제 The diamond problem

1. 문제 :
    - 클래스 A를 상속받은 클래스 B, 클래스 C  (B, C 모두 동일한 A의 메소드를 재정의함)
    - 클래스 B, 클래스 C 를 상속받은 클래스 D
    - 이때, 클래스 D는 B와 C 중 어떤 메소드를 상속하는가?
2. (파이썬에서) 해결 :
    - **MRO(Method Resolution Order)** 알고리즘을 사용하여 클래스 목록을 생성
    - 부모 클래스로부터 상속된 속성들의 검색을 깊이 우선으로, 왼쪽에서 오른쪽으로, 계층 구조에서 겹치는 같은 클래스를 두 번 검색하지 않음
    - 속성이 D에 없으면 B → C 없으면 A

### super()

1. 정의 : 부모 클래스 객체를 반환하는 내장 함수
2. 다중 상속 시 MRO 기반으로 현재 클래스가 상속하는 모든 부모 클래스 중 다음에 호출될 메서드를 결정하여 자동으로 호출
3. 부모 상속할 때에는 self 안써도 됨
4. 사용 사례
    - 단일 상속 구조 : 클래스 이름이 변경되거나 부모 클래스가 교체되어도 super()을 사용하면 코드 수정이더 적게 필요
    - 다중 상속 구조 : MRO를 따른 메서드 호출, 복잡한 다중 상속 구조에서 발생할 수 있는 문제 방지
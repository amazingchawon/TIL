# 에러와 예외

## 디버깅

### 버그 bug

1. 정의 : 소프트웨어에서 발생하는 오류 또는 결함, 프로그램의 예상된 동작과 실제 동작 사이의 불일치

### 디버깅 Debugging

1. 정의 : 소프트웨어에서 발생하는 버그를 찾아내고 수정하는 과정, 프로그래밍의 오작동 원인을 식별하여 수정하는 작업
2. 방법 :
    - print 함수 활용
    - 개발 환경(text editor, IDE) 등에서 제공하는 기능 활용
    - Python tutor 활용 (단순 파이썬 코드인 경우)
    - 뇌 컴파일, 눈 디버깅

## 에러 Error

1. 정의 : 프로그램 실행 중에 발생하는 예외 상황 전체
2. 유형 :
    - 문법 에러 Syntax Error : 프로그램의 구문이 올바르지 않은 경우 발생 (애초에 실행 x)
        - 종류
            - Invaild syntax
            - assign to literal
            - EOL
            - EOF
    - 예외 Exception : 프로그램 실행 중에 감지되는 에러

## 예외

### 내장 예외 Built-in Exceptions

1. ZeroDivisionError : 나누기 또는 모듈로 연산의 두 번째 인자가 0일 때 발생
2. NameError : 지역 또는 전역에서 해당 변수를 찾을 수 없을 때
3. TypeError :
    - 타입 불일치 / 인자 누락 / 인자 초과 / 인자 타입 불일치
4. ValueError : 연산이나 함수에 문제가 없지만 부적절한 값을 인자로 받았고, 상황이 IndexError처럼 더 구체적인 예외로 설명되지 않는 경우 발생
5. IndexError : 시퀀스 인덱스가 범위를 벗어날 때 발생
6. KeyError : 딕셔너리에 해당 키가 존재하지 않는 경우
7. ModuleNotFoundError : 모듈을 찾을 수 없을 때
8. ImportError : import 하려는 이름을 찾을 수 없을 때
9. KeyboardInterrupt : 사용자가 Control-C 또는 Delete를 누를 때 발생 → 무한루프시 강제종료
10. Indentation Error : 잘못된 들여쓰기와 관련된 문법 오류

# 예외처리 Exception Handling

예외가 발생했을 때 프로그램이 비정상적으로 종료되지 않고 적절하게 처리할 수 있도록 하는 방법

## 예외처리 사용 구문

## try & except & else & finally

1. try : 예외가 발생할 수 있는 코드 작성
2. except : 예외가 발생했을 때 실행할 코드 작성
3. else : 예외가 발생하지 않았을 때 실행할 코드 작성
4. finally : 예외 발생 여부와 상관없이 항상 실행할 코드 작성

## 예외 처리 주의사항

1. 내장 예외의 상속 계층구조 :
    - 내장 예외 클래스는 상속 계층구조를 가지기 때문데 except 절로 분기시 반드시 하위클래스를 먼저 확인할 수 있도록 작성해야함
    - VauleError 작동 X
    
    ```python
    try :
    	num = int(input('숫자 입력 : ')
    except BasException : 
    	print(..)
    except ValueError :
    	print(...)
    ```
    

## 예외 객체 다루기

예외 객체 : 예외가 발생했을 때 예외에 대한 정보를 담고 있는 객체

### as 키워드

```python
except IndexError as error :
	print(f'{error}가 발생했습니다.')
```

### try-except 와 if-else

함께 사용 가능

## EAFP & LBYL

### EAFP : Easier to Ask for Forgiveness than Permission

예외처리를 중심으로 코드를 작성하는 접근 방식 (try-except) → 무엇을 검사해야하는지 모르겠을 때

```python
try :
	result = my_dict['key']
	print(result)
except KeyError :
	print('key가 존재하지 않습니다.')
```

### LBYL : Look Before You Leap

값 검사를 중심으로 코드를 작성하는 접근 방식 (if-else) → 조건을 알 때

```python
if 'key' in my_dict :
	result = my_dict['key']
	print(result)
else :
	print('Key가 존재하지 않습니다.')
```
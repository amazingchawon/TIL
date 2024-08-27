# 스택

1. 정의 : 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조
2. 특성 :
    - 선형구조 : 자료 간의 관계가 1대1
    - 비선형구조 : 자료 간의 관계가 1대N (EX) 트리
3. 사용 : 자료를 삽입하거나 꺼냄
    - 마지막에 삽입한 자료를 가장 먼저 꺼냄
    - 후입선출(LIFO, Last-In-First-Out)
4. 연산
    - 삽입 `push` : 저장소에 자료 저장
        
        ```python
        # append 메소드를 통해 리스트의 마지막에 데이터를 삽입
        def push(item):
        	s.append(item)
        ```
        
        ```python
        def push(item, size):
        	global top
        	top += 1
        	if top == size:
        		print('overflow!')
        	else:
        		stack[top] = item
        
        size = 10
        stack = [0] * size
        top = -1
        
        push(10, size)
        top += 1
        stack[top] = 20
        ```
        
    - 삭제 `pop` : 저장소에서 자료를 꺼냄, 꺼낸 자료는 삽입한 자료의 역순
        
        ```python
        def my_pop():
        	if len(s) == 0:
        		# underflow
        		return
        	else:
        		return s.pop()
        ```
        
        ```python
        def my_pop():
        	global top
        	if top == -1:
        		print('underflow')
        		return 0
        	else:
        		top -= 1
        		return stack[top+1]
        ```
        
    - 공백 확인 `isEmpty` : 스택이 공백인지 아닌지 확인
    - top 원소 반환 `peek` : 스택에 top에 있는 item(원소)을 반환

## 스택의 응용

### cf. 스택 구현 고려 사항

1. 1차원 배열을 사용할 경우 : 구현 용이 but, 스택 크기 변경 어려움
    - 해결 방법 : 저장소를 동적으로 할당하여 구현 (동적 연결리스트 이용)
        - 단점 : 구현 복잡
        - 장점 : 메모리 효율적

### 괄호 검사

1. 조건
    - 왼쪽 괄호의 개수와 오른쪽 괄호의 개수가 같아야 함
    - 같은 괄호에서 왼쪽 괄호는 오른쪽 괄호보다 먼저 나와야 함
    - 괄호 사이에는 포함 관계만 존재
2. 구현
    
    ```python
    str_input = '(()())))))'
    top = -1 # paren_stack의 top 위치 기록
    idx = 0 # paren_stack 인덱스
    paren_stack = [0] * len(str_input)  # append 쓰지 않기 위해서
    check = True
    
      for char in str_input:
          if char == '{' or char == '(':      # 여는 괄호가 나오면, [은 문제에서 고려대상이 아님
              paren_stack[idx] = char
              idx += 1
              top += 1
          elif char == '}':  # 닫는 괄호가 직전 여는 괄호와 쌍이 맞는지 확인
              if paren_stack[top] == '{':
                  top -= 1
                  idx -= 1
              else:
                  check = False
                  break
          elif char == ')':  # 닫는 괄호가 직전 여는 괄호와 쌍이 맞는지 확인
              if paren_stack[top] == '(':
                  top -= 1
                  idx -= 1
              else:
                  check = False
                  break
          elif (char in ['}', ')']) and top <= -1:
              check = False
              break
    
      # 순회 완료 후 stack 비어있는지 확인
      if top != -1 or check == False:
          answer = 0
      else:
          answer = 1
    ```
    

### Function call

1. 설명 :
    - 프로그램에서의 함수 호출과 복귀에 따른 수행 순서를 관리
    - 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조 → 스택을 이용하여 수행순서 관리
  
### 계산기

→ 문자열로 된 계산식이 주어질 때, 스택을 이용하여 계산 가능

1. 순서
    - step 1 : 스택을 활용하여 중위 표기법의 수식을 후위 표기법으로 변경
        - 수식의 각 연산자에 대해 우선순위에 따라 괄호를 사용하여 다시 표현
        - 각 연산자를 그에 대응하는 오른쪽 괄호의 뒤로 이동
        - (EX) A*B-C/D → ((AB)*(CD)/)- → AB*CD/-
        - 괄호 제거
            - 참고
                - 중위 표기법 infix notation : 연산자를 피연산자의 가운데로 표기하는 방법 (EX) A + B
                - 후위 표기법 posfix notation : 연산자를 피연산자 뒤에 표기하는 방법 (EX) A B +
    - step 2 : 후위 표기법의 수식을 스택을 이용하여 계산
        - 피연산자를 만나면 스택에 push
        - 연산자를 만나면 필요한 만큼의 피연산자(2개)를 스택에서 pop하여 연산하고, 연산 결과를 다시 스택에 push
            - 처음 pop한 것이 연산자의 오른쪽, 2번째로 pop한 것이 연산자의 왼쪽에 위치하도록 주의
        - 수식이 끝나면, 마지막으로 스택을 pop하여 출력
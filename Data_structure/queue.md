# Queue

1. 특성
    - 스택과 마찬가지로 삽입과 위치가 제한적인 자료구조
    - 선입선출구조 (FIFO)
        - 큐의 뒤에서는 삽입만, 앞에서는 삭제만 이루어지는 구조
        - 큐에 삽입한 순서대로 원소가 저장되어, 가장 먼저 삽입된 원소는 가장 먼저 삭제
2. 선입선출 구조
    - 머리 `Front`
        - 저장된 원소 중 첫번째 원소 (또는 삭제된 위치)
        - 데이터의 삽입이 일어나는 곳
    - 꼬리 `Rear`
        - 저장된 원소 중 마지막 원소
        - 데이터의 삭제가 일어나는 곳

## Queue의 주요 연산

| 연산 | 기능 |
| --- | --- |
| enQueue(item) | 큐의 뒤쪽(rear 다음)에 원소를 삽입하는 연산, `rear += 1` |
| deQueue() | 큐의 앞쪽(front)에서 원소를 삭제하고 반환하는 연산 |
| createQueue() | 공백 상태의 큐를 생성하는 연산, `front = rear = -1` 로 초기화 (큐가 비어있으므로) |
| isEmpty() | 큐가 공백상태인지를 확인하는 연산 |
| isFull() | 큐가 포화상태인지를 확인하는 연산 |
| Qpeek() | 큐의 앞쪽(front)에서 원소를 삭제없이 반환하는 연산 |

## 큐의 구현

### 선형큐

1. 특징:
    - 1차원 배열을 이용한 큐
    - 큐의 크기 = 배열의 크기
    - front : 저장된 첫 번째 원소의 인덱스
    - rear : 저장된 마지막 원소의 인덱스
2. 상태 표현:
    - 초기 상태 : `front = rear = -1`
    - 공백 상태 : `front == rear`
    - 포화 상태 : `rear = n-1` (n : 배열의 크기, n-1 : 배열의 마지막 인덱스)
3. 구현
    - 삽입 `enQueue(item)`
        - 마지막 원소뒤에 새로운 원소를 삽입
        - step 1 : `rear` 값을 하나 증가시켜 새로운 원소를 삽입할 자리를 마련
        - step 2 : 그 인덱스에 해당하는 배열원소 `Q[rear]`에 `item`을 저장
        
        ```python
        def enQueue(item):
        	global rear
        	if self.isFull():
        		print('Queue is full')
        		return
        	else:
        		self.rear += 1 # rear을 다음 위치로 이동
        		self.Q[self.rear] = item
        ```
        
    - 삭제 `deQueue()`
        - 가장 앞에 있는 원소를 삭제
        - step 1 : front 값을 하나 증가시켜 큐에 남아있는 첫 번째 원소로 이동
        - step 2 : 새로운 첫 번째 원소를 리턴 함으로써 삭제와 동일한 기능을 함
        
        ```python
        def deQueue():
        	if self.isEmpty():
        		print('Queue is empty. Cannot dequeue')
        		return None
        	else:
        		self.front += 1
        		return Q[front]
        ```
        
    - 공백상태 및 포화상태 검사 `isEmpty()` `isFull()`
        - 공백상태 : `front == rear`
        - 포화상태 : `rear == n-1`
        
        ```python
        def isEmpty():
        	return front == rear
        	
        def isFull():
        	return rear == len(Q) -1
        ```
        
    - 검색 `Qpeek()`
        - 가장 앞에 있는 원소를 반환하는 연산
        - 현재 front의 한자리 뒤에 있는 원소, 즉 큐의 첫 번째에 있는 원소를 반환
        
        ```python
        def Qpeek():
        	if isEmpty():
        		print("Queue is empty")
        	else:
        		return Q[front+1]
        ```
        
4. 문제점
    - 잘못된 포화 인식
        
        ![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVcbhU%2FbtqZcrZhyi8%2FdxXCV1VLsXznKYpKyKjCOK%2Fimg.png)
        
        - 원소의 삽입과 삭제를 계속할 경우, 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고, rear = n-1상태, 즉 포화 상태로 인식하여 이상의 삽입을 수행하지 않게 됨
    - 해결방법 1
        - 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
        - 원소 이동에 많은 시간이 소요되어 큐의 효율성이 급격히 떨어짐
    - 해결방법 2
        - 1차원 배열을 사용하되, 논리적으로는 배열의 처음과 끝이 연결되어 원형 형태의 큐를 이룬다고 가정하고 사용 → 원형 큐
        

### 원형큐

1. 상태 표현
    - 초기 공백 상태 : `front = rear = 0`
    - index의 순환 :
        - 원형 큐는 배열의 처음과 끝이 연결된 구조 → 큐에 끝에 도달한 다음에는 다시 처음으로 돌아가야 함
        - `(rear+1)%size` 로 나머지 연산을 수행하면 `rear` 가 큐의 크기보다 클 때 처음 위치로 다시 돌아갈 수 있음
    - front 변수 : 공백 상태와 포화상태 구분을 쉽게 하기 위해 front가 있는 자리를 사용하지 않고 항상 빈자리 둠
        
        
        | 큐의 종류 | 삽입 위치 | 삭제 위치 |
        | --- | --- | --- |
        | 선형큐  | rear = rear + 1 | front = front + 1 |
        | 원형큐 | rear = (rear + 1) % n | front = (front + 1) % n |
2. 구현
    - 공백상태 및 포화상태 검사 `isEmpty()` `isFull()`
        - 공백 상태 : `front == rear`
        - 포화 상태 : 삽입할 `rear` 의 다음 위치 == 현재 `front`
            - `(rear+1) % n == front`
        
        ```python
        def isEmpty():
        	return front == rear
        
        def isFull():
        	return (rear+1) % len(cQ) == front
        ```
        
    - 삽입 : `enQueue(item)`
        - 마지막 원소 뒤에 새로운 원소 삽입
        - step 1 : `rear` 값을 조정하여 새로운 원소를 삽입할 자리 마련
        - step 2 : 그 인덱스에 해당하는 배열원소 `cQ[rear]`에 item을 저장
        
        ```python
        def enQueue(item):
        	global rear
        	if isFull():
        		print('Queue is full')
        	else:
        		rear = (rear+1) % len(cQ)
        		cQ[rear] = item
        ```
        
    - 삭제 : `deQueue()` `delete()`
        - 가장 앞에 있는 원소를 삭제하기 위해
        - step 1 : `front` 값을 조정하여 삭제할 자리를 준비
        - step 2 : 새로운 `front` 원소를 리턴함으로써 삭제와 동일한 기능을 함
        
        ```python
        def deQueue():
        	global front
        	if isEmpty():
        		print('Queue is empty. Cannot deQueue')
        	else:
        		front = (front + 1) % len(cQ)
        		return cQ[front] 
        ```
        

### 연결큐

1. 특징
    - 단순 연결 리스트( Linked List)를 이용한 큐
    - 큐의 원소 : 단순 연결 리스트의 노드
    - 큐의 원소 순서 : 노드의 연결 순서, 링크로 연결되어 있음
    - `front` : 첫 번째 노드를 가리키는 링크
    - `rear` : 마지막 노드를 가리키는 링크
    - 상태 표현
        - 초기 상태 : `front = rear = Null`
        - 공백 상태 : `front = rear = Null`

### 참고 deque (덱)

컨테이너 자료형 중 하나

양쪽 끝에서 빠르게 추가와 삭제를 할 수 있는 리스트류 컨테이너

### 우선순위 큐

1. 특징
    - 우선순위를 가진 항목들을 저장하는 큐
    - FIFO 가 아닌, 우선순위가 높은 순서대로 먼저 나가게 됨
2. 우선순위 큐의 적용 분야
    - 시뮬레이션 시스템
    - 네트워크 트래픽 제어
    - 운영체제의 테스크 스케줄링
3. 구현
    - 배열을 이용한 우선순위 큐
        - 설명 :
            - 원소를 삽입하는 과정에서 우선순위를 비교해 적절한 위치에 삽입하는 구조
            - 가장 앞에 최고 우선순위의 원소가 위치하게 됨
        - 문제점 :
            - 배열을 사용하므로, 삽입이나 삭제 연산이 일어날 때 원소의 재배치가 발생
            - 이에 소요되는 시간이나 메모리 낭비가 큼
    - 리스트를 이용한 우선순위 큐

## 큐의 활용

### 버퍼 Buffer

1. 정의 : 데이터를 한 곳에 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역
    - 버퍼링 : 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 의미
2. 버퍼의 자료 구조
    - 버퍼는 일반적으로 입출력 및 네트워크와 관련된 기능에 이용
    - 순서대로 입력, 출력, 전달되어야 하므로 FIFO 방식의 자료구조인 큐가 활용
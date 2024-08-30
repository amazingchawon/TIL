# 비트 연산

1. 용어
    - 1bit : 0과 1을 표현하는 정보의 단위
    - 1 byte : 8bit
2. 파이썬 자료형 용량 (python3 기준)
    - int, string : 28byte
3. 비트 연산
    - 컴퓨터의 CPU는 0과 1로 다루어 동작
    - 내부적으로 비트 연산을 사용하여 덧셈, 뺄셈, 곱셈 등을 계산

## 비트 연산자

| 연산자 | 연산자의 기능 |
| --- | --- |
| AND `&` | a &b → 대응되는 비트가 모두 1일때 1을 반환 |
| OR `\|` | a | b → 대응되는 비트 중에서 하나라도 1이면 1을 반환 |
| XOR `^` | a ^ b → 대응되는 비트가 서로 다르면 1을 반환 |
| NOT `~` | 모든 비트 반전 |
| left shift `<<`  | 특정 수 만큼 비트를 왼쪽으로 밀어냄 |
| right shift `>>` | 특정 수 만큼 비트를 오른쪽으로 밀어냄 (우측 비트들이 제거됨) |
1. 비트 연산 응용
    - `i&(1<<n)`
        - i의 n번째 비트가 1인지 아닌지 확인 가능
        - 1101 & (1<<2) : 1101에서 2번 bit가 1인지 확인 가능 (결과 = 0100)
        - 결과값이 0보다 크면 n번째 비트는 1임이 확정
    - 부분집합 만들기
    
    ```python
    for i in range(1<<len(arr)):
    	for idx in range(len(arr)):
    		if i & (1 <<idx):
    			print(arr[idx], end= ' ')
    	print()
    ```
    

## 음수 표현 방법

### 2의 보수

1. 방법:
    - 반전 후 +1 해주기
        - 0100(2) → 1011 + 1 → 1100(2)
2. 특징
    - 맨 앞자리 bit(MSB)는 음수 or 양수를 구분하는 bit
    - 2의 보수를 취한 수를, 한번 더 2의 보수를 취하면 원래 값으로 돌아옴
3. 2의 보수로 음수를 관리하는 이유
    - 덧셈기 만으로 뺄셈까지 구현 가능 → 속도가 빠르다
    - +0, -0 을 구분하지 않기 위함
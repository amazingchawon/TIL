# Copy
## 복사
파이썬에서는 데이터 분류(가변/불변)에 따라 복사가 달라짐

### immutable
값을 바꿀 수 없는 객체 (bool, int, float, tuple, str)
    ```python
    a = 1
    b = a
    print(a, b) # 1 1
    b = 2
    print(a, b) # 1 2
    ```
### mutable
주소값이 동일하더라도 그 안으 값을 바꿀 수 있는 객체 (list, set, dict)
    ```python
    a = [1, 2, 3, 4]
    b = a
    print(a, b) # [1, 2, 3, 4] [1, 2, 3, 4]
    b[1] = 100
    print(a, b) # [1, 100, 3, 4] [1, 100, 3, 4]
    ```
1. 할당
2. 얕은 복사
    - 정의 : 객체의 참조값(주소값)만 복사하는 것
    - 슬라이싱 : 내부 객체의 주소는 같아서 함께 변경됨
    
    ```python
    a = [1, 2, [3, 4, 5]]
    b = a[:]
    
    b[1] = 10
    print(b) # [1, 10, [3, 4, 5]]
    
    # 얕은 복사의 한계
    b[2][1] = 100
    print(b) # [1, 10, [100, 4, 5]]
    print(a) # [1, 10, [100, 4, 5]]
    ```
    
3. 깊은 복사
    - 정의 : 다른 메모리 공간에 값을 복사하여 mutable 객체의 독립성을 유지해주는 역할
    - copy 모듈의 deepcopy 메소드 사용
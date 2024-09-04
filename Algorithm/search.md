# 검색

1. 정의 : 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업 → 목적하는 탐색 키를 가진 항목을 찾는것
    - 탐색 키 Search Key : 자료를 구별하여 인식할 수 있는 키
2. 종류
    - 순차 검색 sequential search
    - 이진 검색 binary search
    - 해시 hash

## 순차 검색 Sequential Search

1. 정의 : 일렬로 되어 있는 자료를 순서대로 검색하는 방법
    - 가장 간단하고 직관적인 검색 방법
    - 배열(array)나 연결 리스트(linked list) 등 순차구조로 구현된 자료구조에서 원하는 항목을 찾을 때 유용함
    - 알고리즘이 단순하여 구현이 쉽지만, 검색 대상의 수가 많은 경우에는 수행시간이 급격히 증가하여 비효율적임

### 정렬되어 있지 않은 경우

1. 검색 과정
    - 첫 번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는지 비교하며 찾기
    - 키 값이 동일한 원소를 찾으면 그 원소의 인덱스를 반환
    - 자료구조의 마지막에 이를 때까지 검색 대상을 찾기 못하면 검색 실패
2. 정렬되지 않은 자료에서 순차 검색의 평균 비교 회수
    - (n+1)/2
    - 찾고자 하는 원소의 순서에 따라 비교 회수가 결정
    - 시간 복잡도 : O(n)
3. 구현 예
    
    ```python
    # while 문 사용
    def sequential_search (arr, n, key) :
        i = 0
        while i < n and arr[i] != key :
            i += 1
        if i < n :
            return i
        else :
            return -1
    ```
    

### 정렬되어 있는 경우

1. 검색 과정
    - (원소 키 값이 검색 대상의 키보다 작을 때 반복)자료를 순차적으로 검색하면서 키 값을 비교
    - 원소의 키 값이 검색 대상의 키 값보다 크면 원소가 없다는 것 → 검색 종료
2. 정렬되어 있는 자료에서 순차 검색의 평균 비교 회수
    - 평균 비교 회수가 반으로 줄어듦
    - 시간 복잡도 : O(n)
3. 구현 예
    
    ```python
    #  while 문 사용
    def sequential_search2(a, n, key)
    	i = 0
    	while i < n and a[i] < key : # 배열 크기 안에 있고 원소의 키 값이 검색 대상의 키보다 작을 떄
    		i += 1 # index 증가
    	if i < n and a[i] == key :
    		return i
    	else :
    		return -1 # 찾는 키가 없을 경우 -1 반환
    ```
    
    - 주의(필수) : 배열을 탐색할때, 배열을 넘지 않는지 확인하는 코드가 먼저 오게 하기!
        - `while i<n and a[i]<key`
    
## 이진 검색 Binary Search

1. 정의 : 자료의 가운데 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법
2. 특징 : 
    - 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행함
    - 이진 검색을 하기 위해서는 자료가 **정렬**된 상태여야 함
3. 단계 : 
    - 자료의 중앙에 있는 원소 고르기
    - 중앙 원소의 값과 찾고자 하는 목표 값을 비교
    - 목표 값이 중앙 원소 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행, 크다면 자료의 오른쪽 반에 대하여 새로 검색 수행
    - 찾고자 하는 값을 찾을 때까지 과정 반복
4. 시간 복잡도 : O(logN)
5. 구현 :
    - 반복 구조 :
    ```python
    # while 문 사용

    def binary_search (arr, n, key) :
        start = 0 # 시작 원소
        end = n-1 # 마지막 원소
        while start <= end :
            mid = (start + end) // 2 # 몫만 계산
            if arr[mid] == key: # 검색 성공
                return True
            elif arr[mid] > key : # 배열 중간 값이 더 클 경우
                end = mid -1 # mid 앞 부분 검색
            else : # 배열 중간 값이 더 작은 경우
                start = mid + 1 # mid 뒷 부분 검색
        return False # 검색 실패
    ```
    - 재귀 구조 :
    ```python
    def binary_search(arr, low, high, key):
        if low > high
            return -1
        else:
            mid = (low + high) // 2
            if key == arr[mid]:
                return mid
            elif key < arr[mid] :
                return binary_search(arr, low, mid-1, key)
            else:
                return binary_search(arr, mid+1, high, key)
    ```
6. 활용
    - parametric search : 정렬된 데이터를 기준으로 특정 값이나 범위를 검색하는 데 사용
    - Lower Bound, upper Bound :
        - 정렬된 배열에서 특정 값 이상(이하)가 처음으로 나타나는 위치를 찾는 알고리즘
        - 특정 데이터의 범위 검색 등에 활용
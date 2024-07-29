# 정렬

1. 정의 : 2개 이상의 자료를 특정 기준에 의해 작은 값부터 큰 값(오름차순 : asending), 혹은 그 반대의 순서대로(내림차순, descending) 재배열 하는 것
2. 정렬의 종류 : 
    - 버블 정렬 Bubble Sort
    - 카운팅 정렬 Counting Sort
    - 선택 정렬 Selection Sort
    - 퀵 정렬 Quick Sort
    - 삽입 정렬 Insertion Sort
    - 병합 정렬 Merge Sort

## 버블 정렬 Bubble Sort

1. 정의 : 인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식
2. 정렬 과정 :
    - 첫 번째 원소부터 인접한 원소끼리 계속 자리를 교환하면서 맨 마지막 자리까지 이동한다.
    - 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬된다.
    - 교환하며 자리를 이동하는 모습이 물 위에 올라오는 거품 모양과 같다고 하여 버블 정렬이라고 한다.
3. 시간 복잡도 : O(n^2)
4. 그림으로 이해하기
5. Pseudocode
6. 구현
    
    ```python
    def BubbleSort(a, N) : # 정렬할 List, N 원소 수
    	for i in range(N-1, 0, -1) : # 범위의 끝 위치
    		for j in range(0, i) : # 비교할 왼쪽 원소
    			if a[j] > a[j+1] :
    				a[j], a[j+1] = a[j+1], a[j]
    ```
    
    ```python
    def BubbleSort(a, N) :
    	for i in range(n) :
            for j in range(0, n-i-1) :
                if a[j] > a[j+1] :
                    a[j], a[j+1] = a[j+1], a[j]
    ``` 
    
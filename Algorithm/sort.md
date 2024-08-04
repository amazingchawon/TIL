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
    </br>
    ![버블정렬](https://favtutor.com/resources/images/uploads/mceu_61632030011682402256084.png)
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

## 카운팅 정렬 Counting Sort

1. 정의 : 항목들의 순서를 결정하기 위해 집합에 각 항목이 몇 개씩 있는지 세는 작업을 하여, 선형 시간에 정렬하는 효율적인 알고리즘
2. 제한 사항 :
    - 정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능 : 각 항목의 발생 회수를 기록하기 위해, 정수 항목으로 인덱스 되는 카운트들의 배열을 사용하기 때문
    - 카운트들을 위한 충분한 공간을 할당하려면 집합 내의 가장 큰 정수를 알아야 함
3. 시간 복잡도 :
    - O(n+k) : n은 리스트 길이, k는 정수의 최대 값 (k가 백만 이하일때 카운팅 정렬 사용)
4. 절차 :
    </br>
    ![정렬할 배열](https://static.javatpoint.com/ds/images/counting-sort.png)
    - step 1 : 주어진 배열의 최대값(이하 max) 찾기
    </br>![step 1](https://static.javatpoint.com/ds/images/counting-sort2.png)
    - step 2 : 길이가  max+1인 countArray[] 생성 (값은 0으로 초기화)
    </br>![step 2](https://static.javatpoint.com/ds/images/counting-sort3.png)
        - countArray[] : 정렬할 배열에서 값의 등장 횟수를 저장할 array
    
    - step 3 : 정렬할 배열의 값을 인덱스로 사용하여 countArray에 접근하여 값 등장 횟수 증가
    </br>![step 3](https://static.javatpoint.com/ds/images/counting-sort4.png)
    - step 4 : countArray의 인덱스 값을 앞 인덱스 값을 누적하여 저장!
    </br>![step 4-1](https://static.javatpoint.com/ds/images/counting-sort7.png)
    </br>![step 4-2](https://static.javatpoint.com/ds/images/counting-sort8.png)
        - 정렬할 배열의 위치를 정하기 위함
    
    - step 5 : 정렬할 배열의 끝부터 순회 → countArray를 참고해서 순회
    </br>![step 5](https://static.javatpoint.com/ds/images/counting-sort10.png)

5. (step5) 뒤에서부터 정렬하는 이유 → 안정성을 위해
    - 앞에서부터 정렬할 때의 문제 : 주어진 배열에서의 순서가 뒤바뀌게 됨 (1이 arr[0], arr[4], arr[5]에 있다고 가정 → 정렬 후 배열에는 arr[5], arr[4], arr[0] 순으로 정렬된다)
    - (cf) 안정정렬
6. 구현
    ```python
    def counting_sort(arr, k):
        """
        input_arr : 입력 배열 (0 -> K)
        counting_arr : 카운팅 배열
        k는 데이터 개수가 아닌, 데이터 원소 범위

        """
        # 1. 새 배열에 arr 내 원소 빈도수 담기
        counting_arr = [0]*(k+1) # k번째 index가 필요하니 +1
        for i in arr:
            counting_arr[i] += 1
        
        # 2. counting_arr 누적합
        for i in range(1, len(counting_arr)):
            counting_arr[i] = counting_arr[i-1]
        
        # 3. counting_arr 내 기록되어있는 원소 빈도수를 바탕으로 새로운 배열에 정렬
        # 현재 불안정정렬 상태로 정렬됨
        # 안정 정렬로 정렬하고 싶다면 for문 변경 -> for i in ragne(len(result)-1, -1, -1)
        result = [-1] * len(arr)
        for i in arr:
            counting_arr[i] -= 1
            result[counting_arr[i]] = i
        
        return result
    ```

## 선택 정렬 Selection Sort

1. 정의 : 주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하는 방식
2. 정렬 과정 (오름차순 정렬) :
    - 주어진 리스트 중에서 최솟값 찾기
    - 그 값을 리스트의 맨 앞에서 위치한 값과 교환
    - 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위 과정 반복
3. 시간 복잡도 : O(n^2)
4. 구현
    
    ```python
    def selection_sort(arr) :
    	for i in range(len(arr)-1) : # 기준 위치
    		min_idx = i # 현재 구간의 맨 앞을 최소로 가정
    		for j in range(i+1, len(arr)) # 비교 구간
    			if arr[min_idx] > a[j] :
    				min_idx = j
            a[i], a[min_idx] = a[min_idx], a[i]
    	return arr
    ```
    
5. 활용 : k번째로 작은 원소를 찾는 알고리즘
    
    ```python
    def select(arr, k) :
    	for i in range(k) :
    		min_idx = i # 현재 구간의 맨 앞을 최소로 가정
    		for j in range(i+1, len(arr)) # 비교 구간
    			if arr[min_idx] > a[j] :
    				min_idx = j
            a[i], a[min_idx] = a[min_idx], a[i]
    	return arr[k-1]
    ```
    
## 정렬 알고리즘 비교

| 알고리즘 | 평균 수행시간 | 최악 수행시간 | 알고리즘 기법 | 비고 |
| --- | --- | --- | --- | --- |
| 버블 정렬 | O(n^2) | O(n^2) | 비교와 교환 | 가장 손쉬운 코딩 |
| 카운팅 정렬 | O(n+k) | O(n+k) | 비교환 방식 | n이 비교적 작을 때만 가능 |
| 선택 정렬  | O(n^2) | O(n^2) | 비교와 교환 | 교환의 회수가 버블, 삽입정렬보다 작음 |
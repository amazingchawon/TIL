## Union-Find (Disjoint set)

1. 서로소 집합 Disjoint set
    - 정의 : 서로소 또는 상호배타 집합들은 서로 중복 포함된 원소가 없는 집합들
    - 집합에 속한 하나의 특정 멤버를 통해 각 집합들을 구분 → 대표자  representative
2. 상호배타 집합 표현 방법
    - 연결 리스트
        - 같은 집합의 원소들은 하나의 연결리스트로 관리
        - 연결 리스트의 맨 앞 원소를 집합의 대표 원소로 삼기
        - 각 원소는 집합의 대표 원소를 가리키는 링크를 갖음
    - 트리
        - 하나의 집합을 하나의 트리로 표현
        - 자식 노드가 부모노드를 가리키며 루트 노드가 대표자가 됨
    - 일차원 리스트 (아래 구현 방법, 주로 사용하는 방법은 아님)
3. 상호배타 집합 연산
    - 예제:
        
        ```python
        def make_set(n):
        
        def find(x):
        
        def union(x, y):
        
        # 예제
        n = 7
        parents = make_set(n)
        
        union(1, 3)
        union(2, 3)
        union(5, 6)
        
        print('find_set(6)= ', find_set(6)) # 5
        ```
        
    - `Make_Set(x)` : 초기 설정, 유일한 멤버 x를 포함하는 새로운 집합을 생성하는 연산
        
        ```python
        def make_set(n):
        	# 각 원소의 부모를 자신으로 초기화
        	p = [i for i in range(n)]
        	return p
        ```
        
    - `Find_Set(x)` : 대표자가 찾기, x를 포함하는 집합을 찾는 연산
        
        ```python
        def find_set(x):
        	if x == parents[x]:
        		return x
        	else:
        		find_set(parents[x])
        ```
        
    - `Union(x, y)` : 같은 그룹으로 묶기, x와 y를 포함하는 두 집합을 통합하는 연산
        - y의 대표자에 x의 대표자를 넣어줌 → y를 x에 붙여줌
        
        ```python
        def union(x, y):
        	# x와 y의 대표자를 찾자
        	root_x = find_set(x)
        	root_y = find_set(y)
        	
        	# 이미 같은 집합이라면 끝
        	if root_x == root_y:
        		return
        	
        	# 다른 집합이라면 더 작은 루트노트에 합친다
        	# 문제에 따라 다르게 합칠 수 있음
        	if root_x < root_y:
        		parents[y] = root_x
        	else:
        		parents[x] = root_y
        ```
        
4. 문제점 / 해결방법
    - find_set 시, 직접 루트를 가리키는 것이 아니라 비효율적인 연산이 존재
        - Path compression
            - Find-Set을 행하는 과정에서 만나는 모든 노드들이 직접 root를 가리키도록 포인터를 바꿔줌
        - Path compression을 반영한 find_set 구현
            
            ```python
            def find_set(x):
            	if parents[x] == x:
            		return x
            	
            	# 경로 압축
            	# parents[x] : x가 가리키고 있는 부모
            	# find(parents[x]) : x의 부모로부터 대표자를 찾아와라
            	parents[x] = find(parents[x])
            	return parents[x]
            ```
            
    - union 시, 작은 트리를 큰 트리에 붙이는게 효율적
        - Rank를 이용한 Union
            - 각 노드는 자신을 루트로 하는 subtree의 높이를 랭크Rank라는 이름으로 저장
            - 두 집합을 합칠 때 rank가 낮은 집합을 rank가 높은 집합에 붙임
        - Rank를 반영한 Union 구현
            
            ```python
            def make_set(n):
            	p = [i for i in range(n)] # 각 원소의 부모를 자기자신으로 초기화
            	r = [0] * n # 시작 rank 모두 0으로 초기화
            	return p, r
            
            def union(x, y):
            	# rank가 올라가는 시점 : 두개의 rank가 같을 때
            	root_x = find_set(x)
            	root_y = find_set(y)
            	
            	# 이미 같은 집합이라면 끝
            	if root_x == root_y:
            		return
            	
            	# rank를 비교해 더 작은 트리를 큰 트리 밑에 병합
            	if ranks[root_x] > ranks[root_y]:
            		parents[root_y] = root_x
            	elif ranks[root_x] < ranks[root_y]:
            		parents[root_x] = root_y
            	else: # 같다면
            		parents[root_y] = root_x
            		ranks[root_x] += 1
            ```
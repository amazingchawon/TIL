# 최소 비용 신장 트리 (MST)

1. 그래프에서 최소 비용 문제 해결법
    - 모든 정점을 연결하는 간선들의 가중치 합이 최소가 되는 트리 (신장 트리)
    - 두 정점 사이의 최소 비용의 경로 찾기
2. 신장 트리 Spanning Tree
    - 정의 : n개의 정점으로 이루어진 무방향 그래프에서 n개의 정점과 n-1개의 간선으로 이루어진 트리
        - 모든 정점을 연결
        - 간선이 n-1개인
        - 트리 (계층적, 사이클 X)
3. 최소 신장 트리 Minimum Spanning Tree
    - 무방향 가중치 그래프에서 신장 트리를 구성하는 간선들의 가중치의 합이 최소인 신장 트리
4. MST 구현 방법
|  | Prim 알고리즘 | Kruskal 알고리즘 |
| --- | --- | --- |
| 공통점 | 그리디 방식으로 접근 | 그리디 방식으로 접근 |
| 차이점 | 정점 기준 | 간선 기준 |

## Prim 알고리즘

1. 원리 : 하나의 정점에서 연결된 간선들 중 하나씩 선택하면서 MST를 만들어 가는 방식
2. 절차 :
    - 임의 정점을 하나 선택해서 시작
    - 선택한 정점과 인접하는 정점들 중 최소 비용의 간선이 존재하는 정점을 선택
    - 모든 정점이 선택될 때까지 과정 반복
3. 특징 : 서로소인 2개의 집합(2 disjoint-sets) 정보를 유지
    - 트리 정점들(tree vertices) : MST를 만들기 위해 선택된 정점들
    - 비트리 정점들(nontree vertices) : 선택 되지 않은 정점들
4. 구현
    - 인접한 정점들 → BFS 사용
    - 가장 가중치가 작은 것을 먼저 꺼내고 싶다 → heapq(최소힙) 사용
    - 인접행렬 구현 방법
    
    ```python
    from heapq import heappush, heappop
    
    def prim(start):
    	heap = list()
    	MST = [0] * (V) # visited와 같음
    	
    	# 최소 비용 합계
    	sum_weight = 0
    	
    	# 힙에서 관리해야할 데이터
    	# 가중치, 정점 정보 -> 가중치 기준으로 정렬해야하니까 가중치가 먼저 나와야함
    	heappush(heap, (0, start))
    	
    	while heap:
    		weight, v = heappop(heap) # 현재 시점에서 가중치가 가장 작은 정점
    		
    		# 이미 방문한 지점이면 통과
    		if MST[v]:
    			continue
    		
    		# 방문 처리
    		MST[v] = 1
    		sum_weigth += weight
    		
    		# 갈 수 있는 노드를 보면서
    		for next in range(V):
    			# 갈수 없는 지점이면 continue
    			if graph[v][next] == 0:
    				continue
    			
    			# 이미 방문한 지점이면 continue
    			if MST[next]:
    				continue
    				
    			heappush(heap, graph[v][next], next))
    			
    	return sum_weight
    		
    # V : 정점 개수, 0번부터 V-1번까지 존재, E : 간선 수
    V, E = map(int, input().split())
    graph = [[0] * (V) for _ in range(V)] # 인접행렬
    
    # 인접행렬 채우기 : 가중치가 있는 무방향 그래프
    for _ in range(E):
    	u, v, w = map(int, input().split())
    	graph[u][v] = w
    	graph[v][u] = w
    
    result = prim(0)
    print(f'최소 비용 = {result}')
    ```
    
    - 인접리스트 구현 방법
    
    ```python
    # V : 정점 개수, 0번부터 V-1번까지 존재, E : 간선 수
    V, E = map(int, input().split())
    graph = [0 for _ in range(V)] # 인접 리스트
    
    # 인접 리스트 채우기
    for _ in range(E):
    	u, v, w = map(int, input().split())
    	graph[u] = (v, w)
    	graph[v] = (u, w)
    
    result = prim(0)
    print(f'최소 비용 = {result}')
    ```
    

## Kruskal 알고리즘

1. 원리 : 간선을 하나씩 선택해서 MST를 찾는 알고리즘
2. 절차 :
    - 최초, 모든 간선을 가중치에 따라 오름차순 정렬
    - 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴
        - 사이클이 존재하면 다음으로 가중치가 낮은 간선 선택
            - 사이클 판단 여부 → union-find
    - n-1개의 간선이 선택될 때까지 반복
3. 구현 :
    
    ```python
    '''
    7 11
    0 1 32
    0 2 31
    0 5 60
    0 6 51
    1 2 21
    2 4 46
    2 6 25
    3 4 34
    3 5 18
    4 5 40
    4 6 51
    '''
    # V : 정점 개수 (0~V-1번 정점)
    V, E = map(int, input().split())    # V 마지막 정점, 0~V번 정점. 개수 (V+1)개
    edge = []
    for _ in range(E):
        u, v, w = map(int, input().split())
        edge.append([u, v, w])
    edge.sort(key=lambda x : x[2])
    parents = [i for i in range(V)]       # 대표원소 배열
    
    # MST의 간선수 = 정점수(V) - 1
    # cnt : 선택한 간선(edge)의 수, V - 1이 되면 신장트리 완성, 시간 효율을 위해 사용 
    # total : MST 가중치의 합
    cnt = 0
    total = 0
    
    for u, v, w in edge:
    	# 다른 집합이라면, (= 사이클이 없다면)
    	if find_set(u) != fine_set(v):
    		print(u, v, w) # 선택한 순서대로 출력
    		cnt += 1
    		union(u, v)
    		total += w
    		if cnt == V-1:
    			break
    ```
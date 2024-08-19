# 그래프 탐색
비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요
1. 종류 :
    - 깊이 우선 탐색 (Depth First Search)
    - 너비 우선탐색(Breadth First Search)
## BFS (Breadth First Search) 너비 우선 탐색

1. 정의 : 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에, 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식
    - BFS는 그래프나 트리를 탐색하는 알고리즘
    - 시작 노드에서 가까운 노드부터 차례대로 탐색
    - 같은 레벨의 모든 노드를 탐색한 후 다음 레벨로 넘어감
    - 선입선출 FIFO 형태의 자료구조인 큐를 활용함
2. 큐(Queue) 사용 :
    - BFS는 큐 자료구조를 사용하여 구현
    - 큐는 선입선출(FIFO) 방식으로 동작
    - 탐색할 노드를 큐에 넣고, 순서대로 꺼내며 탐색
3. 구현
    - 구현 과정
        - 시작 노드를 큐에 넣고 방문 표시
        - 큐가 빌 때까지 다음 과정을 반복:
            - 큐에서 노드를 하나 꺼낸다.
            - 꺼낸 노드의 인접 노드 중 방문하지 않은 노드를 모두 큐에 넣고 방문 표시를 한다.
    - 방법 1
    
    ```python
    def BFS(GRAPH, vertax):
    	# 준비
    	visited = [0] * (n+1) # n : 정점의 개수
    	queue = [] # 큐 생성
    	queue.append(vertax) # 시작점 v를 큐에 삽입
    	# 탐색
    	while queue: # 큐가 비어있지 않은 경우
    		tmp = queue.pop(0) # 큐의 첫번째 원소 반환
    		if not visited[tmp]: # 방문되지 않은 곳이라면
    			visited[tmp] = True # 방문한 것으로 표시
    			visit(tmp) # 정점 tmp에서 할 일
    			for i in GRAPH[tmp]: # tmp와 연결된 모든 정점에 대해
    				if not visited[i]: # 방문하지 않았다면
    					queue.append(i) # 큐에 넣기
    ```
    
    - 방법 2
    
    ```python
    def BFS(GRAPH, vertax, N):
    	# 준비
    	visited = [0] * (N+1) # visited : 방문 거리 저장 배열
    	queue = [] # 큐 생성
    	queue.append(vertax) # 시작점 vertax를 큐에 삽입
    	visited[v] = 1 # 인큐됨을 표시
    	# 탐색
    	while queue:
    		tmp = queue.pop(0)
    		visit(tmp) # 방문한 정점에서 할일
    		for i in GRAPH[tmp]: # tmp와 연결된 모든 정점에 대해
    			if not visited[i]: # 방문되지 않은 곳이라면
    				queue.append(i) # 큐에 넣기
    				visited[i] = visited[tmp] + 1 # n으로부터 1만큼 이동, 방문거리를 구하는 방법
    ```
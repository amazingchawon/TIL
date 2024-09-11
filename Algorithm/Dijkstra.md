# 최단 경로 알고리즘
1. 최단 경로 정의 : 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로
2. 하나의 시작 정점에서 끝 정점까지의 최단 경로
    - 다익스트라(dijkstra) 알고리즘
        - 음의 가중치를 허용하지 않음
    - 벨만-포드 (Bellan-Ford) 알고리즘
        - 음의 가중치 허용
3. 모든 정점들에 대한 최단 경로
    - 플로이드-워샬(Floyd-Warshall) 알고리즘

# 다익스트라(Dijkstra) 알고리즘

1. 원리 : 시작 정점에서 거리(누적값)가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식
    - 시작 정점(S)에서 끝 정점(T)까지의 최단 경로에 정점 X가 존재
    - 이때, 최단 경로는 S에서 X까지의 최단 경로와 X에서 T까지의 최단 경로로 구성
    - 탐욕 기법을 사용한 알고리즘으로 MST의 프림 알고리즘과 유사
2. 구현
    - BFS → queue 사용
        - 이때, queue에서 먼저 나와야 하는 노드 → 누적값이 적으면 먼저 방문
        - 따라서 heapq 사용
    
    ```python
    '''
    6 8
    0 1 2
    0 2 4
    1 2 1
    1 3 7
    2 4 3
    3 4 2
    3 5 1
    4 5 5
    '''
    
    import heapq
    
    INF = int(1e9)  # 무한을 의미하는 값으로 10억
    
    # n : 노드의 수, m : 간선의 수
    n, m = map(int, input().split())
    # 시작 노드 번호 (문제에 따라 다름)
    start = 0
    # 인접리스트 만들기
    graph = [[] for i in range(n)]
    # 누적거리를 저장할 테이블 - INF 로 저장
    distance = [INF] * n
    
    # 간선 정보를 입력
    for _ in range(m):
        a, b, w = map(int, input().split())
        graph[a].append([b, w])
    
    def dijkstra(start):
        pq = []
        # 시작 노드 최단 거리는 0
        # heapq에 리스트로 저장할 때는 맨 앞의 데이터를 기준으로 정렬된다.
        # heapq에 push 할때는 -> (누적값, 노드번호)
        heapq.heappush(pq, (0, start))
        distance[start] = 0
    
        # 우선순위 큐가 빌 때 까지 반복
        while pq:
            # 가장 최단 거리인 노드에 대한 정보 꺼내기
            dist, now = heapq.heappop(pq)
            # 현재 노드가 이미 처리됐다면 skip
            # 이미 갱신이 되어있다면 -> 넘어가기
            if distance[now] < dist:
                continue
    
            # 현재 노드와 연결된 다른 인접한 노드 확인
            for next in graph[now]:
                next_node = next[0] # 다음 노드
                cost = next[1] # 다음 노드의 가중치
    
                new_cost = dist + cost # 누적 가중치(현재까지의 누적값 + 다음 노드 가중치)
    
                # 다음 노드를 가는 데 더 많은 비용이 드는 경우
                if new_cost >= distance[next_node]:
                    continue
    						
    						# next_node까지 가는데 비용은 new_cost
                distance[next_node] = new_cost
                heapq.heappush(pq, (new_cost, next_node))
    
    # 다익스트라 알고리즘 실행
    dijkstra(start)
    
    # 모든 노드로 가기 위한 최단 거리 출력
    for i in range(n):
        # 도달할 수 없는 경우, 무한 출력
        if distance[i] == INF:
            print("INF", end=' ')
        else:
            print(distance[i], end=' ')
    ```
    
3. 참고
    - 갔다오면 누적값이 커짐 → 최단거리 불가 → 따라서 다익스트라에는 visit 필요 없음
    - 다익스트라 한 번이면, 하나의 정점 → 다른 정점들까지의 최단거리를 모두 구할 수 있음
        - 정점이 0번~N번까지 있고, 0번에서 출발해서 1~N번까지 도달할때 최단 거리가 모두 기록됨
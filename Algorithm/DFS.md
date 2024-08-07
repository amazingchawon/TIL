# 그래프 탐색
비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요
1. 종류 :
    - 깊이 우선 탐색 (Depth First Search)
    - 너비 우선탐색(Breadth First Search)

## DFS (Depth First Search) 깊이 우선 탐색

1. 정의 : 
   - 루트 노드(혹은 임의의 노드)에서 시작해서 다음 분기로 넘어가기 전에 해당 분기를 완벽하게 탐색하는 방식 
   - 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 반복함
2. 구현 방법 종류
   - Stack : 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 스택 사용
   - 재귀
3. 단계
    - 시작 정점 v를 결정하여 방문
    - 정점 v와 인접한 정점 중에서
        - 방문하지 않은 정점 w 존재 : 정점 v를 스택에 push하고 정점 w를 방문. w를 v로 하여 반복
        - 방문하지 않은 정점이 없음 : 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점 v로 하여 다시 반복
    - 스택이 공백이 될때까지 반복
4. 구현
    - pseudo code
    
    ```python
    visited[], stack[] 초기화
    DFS(v)
    	시작점 v 방문
    	visited[v] <- true
    	while {
    		if (v의 인접 정점 중 방문 안 한 정점 w가 있으면)
    			push(v)
    			v <- w (w에 방문)
    			visited[w] <- true
    		else
    			if (스택이 비어 있지 않으면)
    				v <- pop(stack)
    			else
    				break
    	}
    end DFS()
    ```
    - 분기점 중심
    ```python
    def DFS(s, N):              # s : 시작 정점, N: 정점개수
    visited = [0] * (N + 1)     # 방문한 정점을 표시
    stack = []                  # 스택 생성
    print(s)                    # 방문했는지 확인하기 위해 출력
    visited[s] = 1              # 시작 정점 방문 표시
    v = s                       # 현재 정점 v
    
    while True:
        for w in adjL[v]:           # v에 인접하고, 방문 안 한 w가 있으면
            if visited[w] == 0:
                stack.append(v)     # push(v) 현재 정점을 push 하고
                v = w               # w에 방문
                print(v)            # 방문했는지 확인하기 위해 출력
                visited[v] = 1      # w에 방문 표시
                break               # for w, v부터 다시 탐색

        else:                       # 남은 인접 정점이 없어서 break가 걸리지 않은 경우
            if stack:               # if TOP > -1 이전 갈림길을 스택에서 꺼내서
                v = stack.pop()
            else:                   # 되돌아갈 곳이 없으면, 남은 갈림길이 없으면 탐색 종료
                break               # while True:


    '''
    1
    7 8
    1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
    '''
    T = int(input())
    for t in range(1, T + 1):
        # 인접 행렬 만들기
        V, E = map(int, input().split())  # V : 정점개수, E : 간선 수
        adjL = [[] for _ in range(V + 1)]
        arr = list(map(int, input().split()))
        for i in range(E):
            v1, v2 = arr[i * 2], arr[i * 2 + 1]
            adjL[v1].append(v2)
            adjL[v2].append(v1)

    DFS(1, 7)
    ```
5. 인접 정점 기준
    - 인접 행렬 사용
    ```python
    node = ['', 'A', 'B', 'C', 'D', 'E', 'F', 'G']
    
    """
    인접 행렬 (adjacency matrix)
    - n X n 크기의 정사각형 행렬
    - 노드들 간의 연결 관계를 행렬로 표현한 것
    - 무방향 그래프에서는
        - 정점 i와 j 사이에 간선이 있다면, matrix[i][j] = matrix[j][i] =1, 없으면 0
    """

    #       A  B  C  D  E  F  G
    matrix = [
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 1, 1, 0, 0, 0, 0],   # A
        [0, 1, 0, 0, 1, 1, 0, 0],   # B
        [0, 1, 0, 0, 0, 0, 0, 1],   # C
        [0, 0, 1, 0, 0, 0, 1, 0],   # D
        [0, 0, 1, 0, 0, 0, 1, 0],   # E
        [0, 0, 0, 0, 1, 1, 0, 1],   # F
        [0, 0, 0, 1, 0, 0, 1, 0],   # G
    ]

    def DFS(start, Vertax):
        # stack에 시작 정점을 push
        stack = [start]
        # 방문 여부를 체크하는 리스트
        visited = [0] * (Vertax + 1)    # 0번째는 무시, 따라서 +1 해주는 것

        # stack이 빌 때까지 DFS 진행 (stack에 값이 있는 동안 진행)
        while stack:
            # 현재 조사할 노드
            current = stack.pop()

            # 방문하지 않은 노드라면
            if visited[current] == 0:
                visited[current] = 1    # 방문 표시
                print(node[current])    # 방문한 노드 출력 

                # 현재 노드에서 갈 수 있는 다음 노드들을 스택에 추가
                # 탐색 순서를 결정
                # V부터 1까지 역순으로 확인 (작은 번호의 노드가 스택의 위쪽으로 위치하게 됨)
                for next in range(Vertax, 0, -1):
                # 큰 번호 우선 탐색을 한다면 (C부터 탐색)
                # for next in range(1, V+1):
                    # 다음 노드가 간선이 연결이 되어 있고 + 방문한 적이 없다면
                    if matrix[current][next] == 1 and visited[next] == 0:
                        stack.append(next) # stack에 push

    DFS(1, 7)
    ```
    - 인접행렬 만들기
    ```python
    """
    입력
    V E
    V1 V2 V1 V2 ..
    """
    V, E = map(int, input().split()) # V : 노드 개수, E(Edge) : 간선 개수

    adj_matrix = [[0] * (V+1) for _ in range(V+1)]
    data = list(map(int, input().split())) # 노드별 간선 정보

    # 간선 정보 넣기
    # 간선의 개수만큼 반복하면서 넣기
    for i in range(E):
        v1, v2 = data[i * 2], data[i*2 + 1]
        adj_matrix[v1][v2] = 1
        adj_matrix[v2][v1] = 1
    ```


   - stack 풀이 총합
   ```python
   # 정점의 개수, 간선의 개수
   V, E = map(int, input().split())

   # 간선 정보
   data = list(map(int, input().split()))
   print(data)  # [1, 2, 1, 3, 2, 4, 2, 5, 4, 6, 5, 6, 6, 7, 3, 7]

   def DFS(start):
       stack = [start]  # 시작 지점
       visited = [0] * (V + 1)  # 방문 여부를 체크하는 리스트

       # True인 동안 돈다. (==> False 일때까지 돈다.)
       while stack:  # 스택에 값이 있는 동안 반복 (스택이 빌때까지 반복)
           current = stack.pop()  # 현재 조사할 노드

           # 방문했는지 안했는지 (방문하지 않은 노드라면)
           if visited[current] == 0:
               visited[current] = 1  # 방문 표기
               print(current, end= ' ')  # 방문한 노드 출력

               # 현재 노드에서 갈 수 있는 다음 노드들을 찾아야 함 (찾아서 스택에 넣어야 함)
               for next in range(V, 0, -1):
                   # 다음 노드들 중에 
                   # 1. 현재 노드와 간선으로 연결되어 있는지
                   # 2. 방문한 적이 없는지
                   if matrix[current][next] == 1 and visited[next] == 0:
                       stack.append(next)



   # 인접행렬 생성
   matrix = [[0] * (V + 1) for _ in range(V + 1)] # 빈 도화지 만들기

   for i in range(E):
       n1 = data[i * 2]
       n2 = data[i * 2 + 1]
       matrix[n1][n2] = 1
       matrix[n2][n1] = 1


   DFS(1)
   ```

   - 재귀 풀이
     - DFS 재귀함수의 종료 조건
       - 암묵적인 종료 조건
           - 현재 노드에서 더 이상 방문할 수 있는 인접 노드가 없을 때 함수 종료
           - for 반복에서 모든 노드를 검사 했지만 조건을 만족하는 노드가 없다면 종료

   - 동작 과정
   1. DFS(1) 호출
   2. 1번 노드 방문 표시 및 출력
   3. 1번 노드의 인접 노드 중 방문하지 않은 2번 노드 발견
   4. DFS(2)
   5. 2번 노드 방문 표시 및 출력
   6. 2번 노드의 인접 노드 중 방문하지 않은 4번 노드 발견
   7. DFS(4)
   8. ...
   9. 마지막 노드에서 더 이상 방문할 인접 노드가 없음
   10. 이전 호출로 돌아가며 남은 인접 노드 확인
   11. 모든 노드 방문 완료 시 전체 DFS 가 종료
   
   ```python
   # 정점의 개수, 간선의 개수
   V, E = map(int, input().split())

   # 간선 정보
   data = list(map(int, input().split()))
   print(data)  # [1, 2, 1, 3, 2, 4, 2, 5, 4, 6, 5, 6, 6, 7, 3, 7]

   # 인접행렬 생성
   matrix = [[0] * (V + 1) for _ in range(V + 1)]   # 빈 도화지 만들기

   for i in range(E):
       n1 = data[i * 2]
       n2 = data[i * 2 + 1]
       matrix[n1][n2] = 1
       matrix[n2][n1] = 1

   visited = [0] * (V + 1)  # 방문 여부를 체크하는 리스트

   def DFS(start):
       visited[start] = 1       # 방문 기록
       print(start, end= ' ')   # 방문한 노드를 출력

       # 다음으로 조사가 가능한 노드 찾기
       for next in range(1, V + 1):
           # 현재 노드에 인접해있고, 방문한 적이 없는 곳
           if matrix[start][next] == 1 and visited[next] == 0:
               # 또다시 탐색 시작
               DFS(next)

   DFS(1)
   ```
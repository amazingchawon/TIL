# 그래프

1. 정의 : 아이템(사물 또는 추상적 개념)들과 이들 사이의 연결 관계를 표현
    - 코드에서 그래프 : 정점(Vertex)들의 집합과 이들을 연결하는 간선(Edge)들의 집합으로 구성된 자료구조
    - 활용 : 선형 자료구조나 트리 자료구조로 표현하기 어려운 N : N 관계를 가지는 원소들을 표현하기에 용이

## 그래프 유형

[그래프의 표현에 따른 구분]

1. 무향 그래프 Undirected Graph
    - 예시 : 친구
2. 유향 그래프 Directed Graph
    - 예시 : 지하철, 도로, 팔로우 등
3. 가중치 그래프 Weighted Graph
    - 예시 : 도로건설(비용), 비행기(금액)
    - 가중치 그래프는 무향일까, 유향일까? 둘 다 가능하다
4. 사이클 없는 방향 그래프 DAG(Directed Acyclic Graph)

[그래프의 연결에 따른 구분]

1. 완전 그래프 : 정점들에 대해 가능한 모든 간선들을 가진 그래프
2. 부분 그래프 : 완전 그래프에서 일부 정점이나 간선을 제외한 그래프

## 용어

| 인접 Adgacency | 두 개의 정점에 간선이 존재하면 서로 인접해 있다고 함 |
| --- | --- |
| 경로 | 간선을 특정한 순서대로 나열한 것 |
| 단순 경로 | 한 정점을 최대 한번만 지나가는 경로 |
| 사이클 Cycle | 시작한 정점에서 끝나는 경로 |

## 그래프 표현

간선의 정보를 저장하는 방식, 메모리나 성능을 고려해서 결정

1. 인접 행렬 Adjacent Matrix
    - V x V 크기의 2차원 배열을 이용해서 간선 정보를 저장
        - 연결이 안되어있다는 정보도 함께 저장
    - 장점 : 연결 여부를 한 번에 탐색 가능
    - 단점 : 메모리 낭비가 심함
2. 인접 리스트 Adjacent List
    - 각 정점마다 해당 정점으로 나가는 간선의 정보를 저장
        - 연결이 되어있다는 정보만 저장
        - 예시 : A : [B, C], B : [A], C : [A]
    - 장점 : 메모리 활용이 효율적
    - 단점 : 연결 여부를 한 번에 탐색 불가 (순회 필수)
3. 간선의 배열
    - 간선(시작 정점, 끝 정점)을 배열에 연속적으로 저장
4. 인접 리스트

## 그래프 알고리즘
### 탐색
1. 정의 : 비선형구조인 그래프로 표현된 모든 자료(정점)를 빠짐없이 탐색하는 것
2. 종류
    - [DFS](https://github.com/amazingchawon/TIL/blob/master/Algorithm/DFS.md) : 깊이 우선 탐색
        - 문제 키워드 : 끝까지 전달을 해봐야 한다
    - [BFS](https://github.com/amazingchawon/TIL/blob/master/Algorithm/BFS.md) : 너비 우선 탐색
        - 문제 키워드 : 퍼져나가면서 해결

### [Union-Find (Disjoint set)](https://github.com/amazingchawon/TIL/blob/master/Algorithm/Union-Find.md)

### [최소 비용](https://github.com/amazingchawon/TIL/blob/master/Algorithm/MST.md)
1. 그래프에서 최소 비용 문제 해결법
    - 모든 정점을 연결하는 간선들의 가중치 합이 최소가 되는 트리 (신장 트리)
    - 두 정점 사이의 최소 비용의 경로 찾기
2. 종류
   - Prim 알고리즘
     - 그리디 방식으로 접근
     - 정점 기준
   - Kruskal 알고리즘
     - 그리디 방식으로 접근
     - 간선 기준

## 최단 경로
1. 최단 경로 정의 : 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로
2. 종류 :
   - 하나의 시작 정점에서 끝 정점까지의 최단 경로
      - [다익스트라(dijkstra) 알고리즘](https://github.com/amazingchawon/TIL/blob/master/Algorithm/Dijkstra.md)
          - 음의 가중치를 허용하지 않음
      - 벨만-포드 (Bellan-Ford) 알고리즘
          - 음의 가중치 허용
   - 모든 정점들에 대한 최단 경로
      - 플로이드-워샬(Floyd-Warshall) 알고리즘
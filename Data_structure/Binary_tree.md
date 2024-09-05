# 이진트리 **Binary Tree**

1. 정의
    - 모든 노드들이 최대 2개의 서브트리를 갖는 특별한 형태의 트리
    
    ![이진 트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblbjFV%2Fbtq1K3P9Y8v%2FH393OwoRI9lX8N3wrz9OO1%2Fimg.png)
    
2. 특성
    - 레벨 i에서 최대 노드 개수 : 2^i
    - 높이가 h인 이진 트리
        - 노드의 최소 개수 : h + 1
        - 노드의 최대 개수 : 2^(h+1) - 1

## 이진 트리의 종류

![이진 트리의 종류](https://velog.velcdn.com/images/kwontae1313/post/1ec18554-1789-4831-acb0-5a2efdb4834b/image.png)

1. 포화 이진 트리 Full Binary Tree
    - 모든 레벨에 노드가 포화상태로 차 있는 트리
    - 높이가 h 일때, 최대 노드 개수인 2^(h+1) - 1 개의 노드를 가진 이진 트리
2. 완전 이진 트리 Complete Binary Tree
    - 높이가 h이고 노드 수가 n개 일 때 (단, 2^h ≤ n < 2^(h+1) -1), 포화 이진 트리의 노드 번호 1번부터 n번까지 **빈 자리가 없는** 이진 트리
    - 왼쪽에서부터 채워져 있는 이진 트리
        - 마지막 레벨을 제외하고는 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 경우 왼쪽 부터 채워져 있는 트리
3. 편향 이진 트리 Skewed Binary Tree
    - 높이 h에 대한 최소 개수의 노드를 가지면서 한쪽 방향의 자식 노드만을 가진 이진 트리
        - 왼쪽 편향 이진 트리
        - 오른쪽 편향 이진 트리

## 이진 트리의 순회 Traversal

1. 순회
    - 트리의 각 노드를 중복되지 않게 전부 방문(visit)하는 것
    - 트리의 노드들을 체계적으로 방문하는 것
2. 순회 방법
    
    ![순회 방법](https://velog.velcdn.com/images/yeonkr/post/ee60ce12-78c6-4971-b47a-4dc3f12cc60d/image.webp)
    
    - 전위 순회 preorder traversal : VLR
        - 부모 노드 - 왼쪽 자식 노드 - 오른쪽 자식 노드
    - 중위 순회 inorder traversal : LVR
        - 왼쪽 자식 노드 - 부모 노드 - 오른쪽 자식 노드
    - 후위 순회 postorder traversal : LRV
        - 왼쪽 자식 노드 - 오른쪽 자식 노드 - 부모 노드

### 전위 순회 Preorder Traversal

1. 수행 방법
    - 현재 노드 n을 방문하여 처리
    - 현재 노드 n의 왼쪽 서브트리로 이동
    - 현재 노드 n의 오른쪽 서브트리로 이동
2. 알고리즘
    
    ```python
    def preorder_traverse(TREE):
    	if TREE:
    		visit(TREE)
    		preorder_traverse(TREE.left)
    		preorder_traverse(TREE.right)
    ```
    

### 중위 순회 Inorder Traversal

1. 수행 방법
    - 현재 노드 n의 왼쪽 서브트리로 이동
    - 현재 노드 n을 방문하여 처리
    - 현재 노드 n의 오른쪽 서브트리로 이동
2. 알고리즘
    
    ```python
    def inorder_traverse(TREE):
    	if TREE :
    		inorder_traverse(TREE.left)
    		visit(TREE)
    		inorer_traverse(TREE.right)
    ```
    

### 후위 순회 Postorder Traversal

1. 수행 방법
    - 현재 노드 n의 왼쪽 서브트리로 이동
    - 현재 노드 n의 오른쪽 서브트리로 이동
    - 현재 노드 n을 방문하여 처리
2. 알고리즘
    
    ```python
    def postorder_traverse(TREE):
    	if TREE:
    		postorder_traverse(TREE.left)
    		postorder_traverse(TREE.right)
    		visit(TREE)
    ```
    

## 이진 트리의 표현

### 배열 활용

![배열 활용](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtadRz%2Fbtq1KAU3krj%2FkYPmEpCTVoginhaKXxcA8K%2Fimg.png)

1. 레벨 n에 있는 노드에 대하여 왼쪽부터 오른쪽으로 2^n 부터 2^(n+1)-1까지 번호를 차례로 부여
2. 루트 노트의 인덱스 i가 0인 경우
    - 왼쪽 자식 노드 : 2*i + 1 번째 노드
    - 오른쪽 자식 노드 : 2* i + 2 번째 노드
    - 노드 i의  부모 → (i-1)//2 번째 노드
3. 루트 노트의 인덱스 i가 1인 경우
    - 왼쪽 자식 노드 : 2*i 번째 노드
    - 오른쪽 자식 노드 : 2*i + 1 번째 노드
    - 노드 i의 부모 → i//2번째 노드
    - 레벨 n의 노드 시작 번호 → 2^n
4. 배열 구현 방식의 장단점
    - 장점 : 노드 접근 빠름, 구현이 용이
    - 단점 :
        - 편향 이진트리 같은 경우 많은 공간 낭비
        - 노드 삽입, 삭제가 어려움
          - (코테) 삭제가 발생하는 문제 : 1차원 리스트 사용하지 말기!

### 연결 리스트 활용 (개발, 정석적)

1. 연결 자료구조를 이용하 이진트리의 표현
    - 이진 트리의 모든 노드는 최대 2개의 자식 노드를 가지므로, 일정한 구조의 단순 연결 리스트 노드를 사용하여 구현

```python
from collections import deque

class TreeNode:
    def __init__(self, key):
        self.key = key  # 노드의 값
        self.left = None  # 왼쪽 자식 노드를 가리킴
        self.right = None  # 오른쪽 자식 노드를 가리킴

class BinaryTree:
    def __init__(self):
        self.root = None  # 트리의 루트 노드

    # 새로운 노드를 삽입하는 함수 (레벨 순서 삽입)
    def insert(self, key):
        new_node = TreeNode(key)
        if self.root is None:
            self.root = new_node
            return

        # 레벨 순서로 트리를 탐색하기 위해 큐를 사용
        queue = deque([self.root])

        while queue:
            node = queue.popleft()

            # 왼쪽 자식이 비어있으면 삽입
            if node.left is None:
                node.left = new_node
                break
            else:
                queue.append(node.left)

            # 오른쪽 자식이 비어있으면 삽입
            if node.right is None:
                node.right = new_node
                break
            else:
                queue.append(node.right)

    def inorder_traversal(self):
        # 중위 순회를 통해 트리의 노드들을 출력하는 함수
        return self._inorder_traversal(self.root, [])

    def _inorder_traversal(self, node, result):
        if node:
            self._inorder_traversal(node.left, result)
            result.append(node.key)
            self._inorder_traversal(node.right, result)
        return result

# 예제 사용법
if __name__ == "__main__":
    tree = BinaryTree()
    tree.insert(50)
    print("Inorder Traversal:", tree.inorder_traversal())
    tree.insert(30)
    print("Inorder Traversal:", tree.inorder_traversal())
    tree.insert(20)
    print("Inorder Traversal:", tree.inorder_traversal())
    tree.insert(40)
    print("Inorder Traversal:", tree.inorder_traversal())
    tree.insert(70)
    print("Inorder Traversal:", tree.inorder_traversal())
    tree.insert(60)
    print("Inorder Traversal:", tree.inorder_traversal())
    tree.insert(80)

    print("Inorder Traversal:", tree.inorder_traversal())

```

### 인접 리스트 활용 (코테)

```python
'''
13
1 2 1 3 2 4 3 5 3 6 4 7 5 8 5 9 6 10 6 11 7 12 11 13
'''

def dfs(node):
    if node == -1:
        return

    preorder.append(node)
    dfs(graph[node][0])
    inorder.append(node)
    dfs(graph[node][1])
    postorder.append(node)

N = int(input())
E = N - 1
arr = list(map(int, input().split()))
graph = [[] for _ in range(N + 1)]
# append 를 통해 갈 수 있는 경로를 추가하기
for i in range(E):
    p, c = arr[i * 2], arr[i * 2 + 1]
    graph[p].append(c)

# 없는 경우 -1로 데이터를 저장하기 위한 코드("좌우 경로가 있는가 ?")
# 탐색 시 index 오류를 방지하기 위해 없는 경로를 -1로 저장하였습니다.
for i in range(N + 1):
    while len(graph[i]) < 2:
        graph[i].append(-1)

preorder = []
inorder = []
postorder = []

dfs(1)

print(*inorder)
print(*preorder)
print(*postorder)
```

## (cf) 이진 트리의 저장

1. 부모 번호를 인덱스로 자식 번호를 저장
    - 부모를 인덱스로 사용하고 left, right 배열에 각각 왼쪽 자식, 오른쪽 자식 저장
    - root node 알 수 X
2. 자식 번호를 인덱스로 부모 번호를 저장
3. 위 배열로 루트 찾기, 조상 찾기

```python
'''
13 # 정점의 개수
1 2 1 3 2 4 3 5 3 6 4 7 5 8 5 9 6 10 6 11 7 12 11 13 # 부모 - 자식
'''

# 전위 순회 (나 -> 왼쪽 -> 오른쪽)
def pre_order(T):
    if T:
        print(T, end = ' ')
        pre_order(left[T])
        pre_order(right[T])

# 단, 입력이 반드시 각 노드당 최대 2번씩만 들어온다고 가정한 코드
N = int(input())        # 1번부터 N번까지인 정점
E = N-1                 # E : 간선, 
arr = list(map(int, input().split()))
# ex ) left[3] = 2 ==> 3번 부모의 왼쪽 자식은 2다.
left = [0]*(N+1)        # 부모를 인덱스로 왼쪽 자식 번호 저장
right = [0]*(N+1)       # 부모를 인덱스로 오른쪽 자식 번호 저장
par = [0]*(N+1)         # 자식을 인덱스로 부모 저장

for i in range(E):
    p, c = arr[i*2], arr[i*2+1]
# for i in range(0,E*2, 2):
#         p, c = arr[i], arr[i + 1]
    if left[p]==0:          # 왼쪽자식이 없으면
        left[p] = c         # 왼쪽에 삽입
    else:
        right[p] = c
    par[c] = p

c = N
while par[c]!=0:        # 부모가 있으면
    c = par[c]          # 부모를 새로운 자식으로 두고
root = c                # 더이상 부모가 없으면 root
print(root)
pre_order(root)
```
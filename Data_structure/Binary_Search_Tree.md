# 이진 탐색 트리 BST : Binary Search Tree

1. 정의 :
    - Data들을 빠르게 검색할 수 있도록 체계적으로 저장 해 두고, 최대 O(logn)(진수 2)의 빠른 속도로 값을 검색할 수 있는 자료구조
    - 탐색 작업을 효율적으로 하기 위한 자료구조
    - 빠르게 검색될 수 있도록, 특정 규칙을 갖는 이진트리 형태로 값을 저장
        - 규칙 : 부모노드보다 크면 오른쪽 자식에, 작으면 왼쪽 자식에 삽입
2. 특징
    - 대부분의 경우, 모든 원소는 서로 다른 유일한 키를 갖는다
    - 왼쪽 서브트리와 오른쪽 서브트리도 이진 탐색 트리
    - 중위 순회하면 오름차순으로 정렬된 값을 얻을 수 있음
3. 리스트 VS BST 성능 비교
    
    
    |  | 리스트 | BST |
    | --- | --- | --- |
    | 삽입 | O(n), 단 맨 끝 삽입은 O(1) | 평균 O(logN) |
    | 삭제 | O(n), 단 맨 끝 삭제는 O(1) | 평균 O(logN) |
    | 탐색 | O(n) | 평균 O(logN) |
    - BST 성능
        - 탐색, 삽입, 삭제 시간은 트리의 높이만큼 시간이 걸림
        - 평균의 경우 O(logn) : 이진트리가 균형적으로 생성되어 있는 경우
        - 최악의 경우 O(n) : 한쪽으로 치우친 경사 이진트리의 경우

## BST 동작 원리 - 삽입/탐색

1. root부터 바닥 노드까지 탐색을 하며 자기 위치를 찾음

## BST 동작 원리 - 삭제

1. 조건 : 노드를 삭제해도, BST 규칙을 유지해야함
2. 방법 : 자식의 개수에 따라 다르게 동작
    - 자식이 없는 노드 (Leaf node) : 그냥 삭제해도 무방
    - 자식이 1개 있는 노드 : 삭제하고자 하는 노드의 자식 트리를 본인의 부모 노드에 연결 후 삭제
    - 자식이 2개 있는 노드 :
        - 삭제하고자 하는 노드의 왼쪽 서브 트리의 가장 큰 값
        - 삭제하고자 하는 노드의 오른쪽 서브 트리의 가장 작은 값
3. 시간 복잡도 : 평균 O(logN)
    - 지우려고 하는 노드를 탐색하고 삭제 해야하기 때문

## 구현

```python
'''
7
3 5 1 2 7 4 -5
'''

# 노드
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

# BST
class BinarySearchTree:
    def __init__(self):
        self.root = None

    # 삽입
    def insert(self, key):
        if self.root is None:                       # root 노드 비어있으면,
            self.root = Node(key)                   # root 노드 추가
        else:
            self._insert(self.root, key)            # root 노드의 key와 현재 추가하는 key 비교해서 삽입

    def _insert(self, node, key):
        if key < node.key:                          # 왼쪽 트리로 : 현재 입력한 값이 기록된 값보다 작다면,
            if node.left is None:                   # 비어 있다면,
                node.left = Node(key)               # 추가
            else:
                self._insert(node.left, key)        # 비어 있지 않다면, 재귀 호출
        else:                                       # 오른쪽 트리로
            if node.right is None:
                node.right = Node(key)
            else:
                self._insert(node.right, key)

    # 탐색
    def search(self, key):
        return self._search(self.root, key)         # root 노드부터 탐색

    def _search(self, node, key):
        if node is None or node.key == key:         # 일치하면,
            return node                             # 반환
        if key < node.key:                          # 왼쪽 트리로 : 현재 찾는 값이 기록된 값보다 작다면,
            return self._search(node.left, key)     # 재귀 호출
        else:
            return self._search(node.right, key)

    # 순회
    # 중위순회하면 오름차순으로 정렬된 값을 얻을 수 있음
    def inorder_traversal(self):
        self._inorder_traversal(self.root)

    def _inorder_traversal(self, node):
        if node:
            self._inorder_traversal(node.left)
            print(node.key, end=' ')
            self._inorder_traversal(node.right)
    
    
    # 삭제
    def delete(self, key):
        self.root = self._delete(self.root, key)

    def _delete(self, node, key):
        # 기본 경우: 트리가 비어있거나 키를 찾지 못한 경우
        if node is None:
            return node

        # 키를 찾아 왼쪽 또는 오른쪽 서브트리로 이동
        if key < node.key:
            node.left = self._delete(node.left, key)
        elif key > node.key:
            node.right = self._delete(node.right, key)
        else:
            # 키를 찾은 경우: 이 노드를 삭제해야 함

            # 경우 1: 리프 노드 또는 하나의 자식만 있는 노드
            if node.left is None:
                return node.right
            elif node.right is None:
                return node.left

            # 경우 2: 두 개의 자식이 있는 노드
            # 오른쪽 서브트리에서 가장 작은 노드를 찾음
            temp = self._min_value_node(node.right)
            # 현재 노드의 키를 후계자의 키로 대체
            node.key = temp.key
            # 후계자 노드를 삭제
            node.right = self._delete(node.right, temp.key)

        return node

N = int(input())
arr = list(map(int, input().split()))

bst = BinarySearchTree()

for num in arr:
    bst.insert(num)

print("중위 순회 결과:", end=' ')
bst.inorder_traversal()  # 중위 순회: -5 1 2 3 4 5 7

# 탐색 예제
key = 5
result = bst.search(key)
if result:
    print(f"\n키 {key} 찾음.")
else:
    print(f"\n키 {key} 못 찾음.")

```
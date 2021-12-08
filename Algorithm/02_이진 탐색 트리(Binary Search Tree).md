# 트리(Tree)

## 정의

### 트리는 가계도와 같은 계층적인 구조를 표현할 때 사용할 수 있는 자료구조

![자료구조(Data structure) - 트리(tree)의 기본 개념/용어 : 네이버 블로그](https://mblogthumb-phinf.pstatic.net/20140219_285/4717010_13927847504767z2aF_PNG/350px-Sorted_binary_tree_svg.png?type=w2)

### [트리 관련 용어]

- 루트 노드(root node) : 부모가 없는 최상위 노드
- 단말 노드(leaf node) : 자식이 없는 노드
- 크기(size) : 트리에 포함된 모든 노드의 개수
- 깊이(depth) : 루트 노드부터의 거리 ex) 루트 노드의 깊이는 0
- 높이(height) : 깊이 중 최댓값
- 차수(degree) : 각 노드의 (자식 방향) 간선 개수

- 트리의 크기가 N일 때, 전체 간선의 개수는 **N - 1**개 입니다.

### 이진 탐색 트리(Binary Search Tree)

![알고리즘 2-3강. 탐색 알고리즘 - 이진 탐색 트리(Binary Search Tree)](https://t1.daumcdn.net/cfile/tistory/2321CB4951A467AC0B)

#### 이진 탐색 트리의 특징

- 왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드
  - 부모 노드보다 왼쪽 자식 노드가 작다
  - 부모 노드보다 오른쪽 자식 노드가 크다

#### 데이터 조회 과정

위에 트리에서 14를 찾는다고 할때

1. 루트 노드 방문(10)
2. 10이 14보다 작으니까 오른쪽으로 이동 -> 루트 노드에서 왼쪽 루트는 전혀 볼필요 없음
3. 오른쪽 트리에서 17과 14를 비교해서 14가 17보다 작으므로 왼쪽 트리로 이동
4. 14를 찾았으므로  탐색 종료

#### 주의!

이진 탐색 트리의 탐색 시간은 O(logN)인데 이렇게 빠르게 탐색할 수 있는 경우는 트리의 모양이 왼쪽과 오른쪽이 균형잡힌 형태 일때만 빠르게  탐색 가능



### 트리의 순회(Tree Traversal)

트리 자료구조에 포함된 노드를 특정한 방법으로 한 번씩 방문하는 방법

#### 대표적인 트리 순회 방법

1. 전위 순회(pre-order traverse) : 루트를 먼저 방문
   - 10 -> 5 -> 1 -> 6 -> 17 -> 14 -> 21
2. 중위 순회(in-order traverse) : 왼쪽 자식을 방문한 뒤에 루트를 방문
   - 1 -> 5 -> 6 -> 10 -> 14 -> 17 -> 21
3. 후위 순회(post-order traverse) : 오른쪽 자식을 방문한 뒤에 루트를 방문
   - 1 -> 6 -> 5 -> 14 -> 21 -> 17 -> 10

#### 트리의 순회(Tree Traversal) 구현 예제

```python
# 노드 클래스 생성
class Node:
    def __init__(self, data, left_node, right_node):
        self.data = data
        self.left_node = left_node
        self.right_node = right_node

# 전위 순회(Preorder Traversal)
def pre_order(node):			# 루트 노드를 인자로 받아온다.
    print(node.data, end=' ')	# 부모 노드 출력
    if node.left_node != None:	# 부모 노드의 왼쪽 노드가 != None면 
        pre_order(tree[node.left_node])		# 그 노드를 부모 노드로 다시 pre_order함수 실행
    if node.right_node != None:				# 왼쪽 노드가 None이면 오른쪽 노드를 검사
        pre_order(tree[node.right_node])	# 오른쪽 노드를 부모 노드로 해서 pre_order함수 실행
        
# 중위 순회(Inorder Traversal)
def in_order(node):
    if node.left_node != None:
        in_order(tree[node.left_node])
    print(node.data, end=' ')
    if node.right_node != None:
        in_order(tree[node.right_node])

# 후위 순회(Postorder Traversal)
def post_order(node):
    if node.left_node != None:
        post_order(tree[node.left_node])
    if node.right_node != None:
        post_order(tree[node.right_node])
    print(node.data, end=' ')
 
n = int(input())	# 트리의 크기 입력
tree = {}			# dict 구조
# A ~ G까지 루트 노드부터 왼쪽 -> 오른쪽으로 트리가 2차수인 트리 생성
for i in range(n):
    data, left_node, right_node = input().split()	#부모 노드, 자식 노드1, 자식 노드2 입력
    # 자식 노드가 없으면 None
    if left_node == "None":
        left_node = None
    if right_node == "None"
    	right_node = None
    # 트리 리스트에 노드 삽입
    tree[data] = Node(data, left_node, right_node)
    
pre_order(tree['A'])
print()
in_order(tree['A'])
print()
post_order(tree['A'])
```
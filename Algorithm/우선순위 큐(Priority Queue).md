# 우선순위 큐(Priority Queue)

## 우선순위가 가장 높은 데이터를 가장먼저 삭제하는 자료구조

- ### 우선순위 큐를 구현하는 방법

1. 리스트

   1. 삽입시간 - O(1)
   2. 삭제시간 - O(N) 선형적인 탐색시간이 소요

2. 힙(heap) 

   - 완전 이진 트리 자료구조의 일종으로 항상 **루트 노드**를 제거한다.
   - 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조

   1. 삽입시간 - O(logN)
   2. 삭제시간 - O(logN)

- ### Heapify()

  1. min - heap

     - 부모 노드로 거슬러 올라가며, 부모보다 자신의 값이 더 작은 경우 교체

  2. max - heap

     1. 왼쪽 자식의 인덱스 = a[2*k + 1]
        오른쪽 자식의 인덱스 = a[2\*k + 2]
        부모의 인덱스 = (자식의 인덱스) / 2 - 1

     2. ![img](https://gmlwjd9405.github.io/images/data-structure-heap/heap-index-parent-child.png)출처 https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html

     3. 삽입( 8 )

        1. 인덱스 순으로 가장 마지막에 위치에 이어서 새로운 요소 8을 삽입
        2. 부모 노드 4 < 삽입 노드 8 이므로 서로 교환
        3. 부모 노드 7 < 삽입 노드 8 교환
        4. 부모 노드 9 > 삽입 노드 8 교환x

     4. 삭제

        1. 루트 노드 9 삭제 (빈자리에는 최대 힙의 마지막(index) 노드를 가져온다.)
        2. 삽입 노드와 자식 노드를 비교하여 자식 노드 중 더 큰 값과 교환(7 > 6)
        3. 삽입 노드와 자식 노드를 비교하여 자식 노드 중 더 큰 값과 교환(5 > 4)
        4. 자식 노드 1, 2 < 삽입 노드 3 이므로 교환x

     5. ```python
        import sys
        import heapq	#heap 라이브러리 제공
        input = sys.stdin.readline
        
        # min heap(python기준)
        def heapsort(iterable):
            h = []
            result = []
            # 모든 원소를 차례대로 힙에 삽입, 루트노드 기준으로 왼쪽에서 오른쪽으로 레벨순으로 삽입
            for value in iterable:
                heapq.heappush(h, value)	#heapq.heappush(heap, item)
            # 힙에 삽입된 모든 원소를 차례대로 꺼내어 담기
            for _ in range(len(h)):
                result.append(heapq.heappop(h))	#heapq.heappop(heap) 가장 작은 항목 삭제
            return result
        
        n = int(input())
        arr = []
        
        for i in range(n):
            arr.append(int(input()))
            
        res = heapsort(arr)
        
        for i in range(n):
            print(res[i], end=' ')
        ```
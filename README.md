# assignment05

## 3. heap
```python
import csv
from datetime import datetime

class Heap:
    def __init__(self, *args):
        if len(args) != 0:
            self.__A = args[0]
        else:
            self.__A = []

    def insert(self, x):
        self.__A.append(x)
        self.__percolateUp(len(self.__A)-1)

    def __percolateUp(self, i: int):
        parent = (i - 1) // 2
        if i > 0 and self.__A[i] > self.__A[parent]:
            self.__A[i], self.__A[parent] = self.__A[parent], self.__A[i]
            self.__percolateUp(parent)

    def deleteMax(self):
        if not self.isEmpty():
            max = self.__A[0]
            self.__A[0] = self.__A.pop()
            self.__percolateDown(0)
            return max
        else:
            return None

    def __percolateDown(self, i: int):
        child = 2 * i + 1
        right = 2 * i + 2
        if child <= len(self.__A)-1:
            if right <= len(self.__A)-1 and self.__A[child] < self.__A[right]:
                child = right
            if self.__A[i] < self.__A[child]:
                self.__A[i], self.__A[child] = self.__A[child], self.__A[i]
                self.__percolateDown(child)

    def max(self):
        return self.__A[0]

    def buildHeap(self):
        for i in range((len(self.__A) - 2) // 2, -1, -1):
            self.__percolateDown(i)

    def isEmpty(self) -> bool:
        return len(self.__A) == 0

    def clear(self):
        self.__A = []

    def size(self) -> int:
        return len(self.__A)


heap = Heap()


with open(r'C:\python09\DS_Birthdaydata.csv', newline='', encoding='cp949') as csvfile: # 2. CSV 파일 열기 (공란 생일은 제외)
    reader = csv.DictReader(csvfile)
    for row in reader:
        name = row['이름']
        birthday_str = row['생일'].strip()

        # 생일이 공란이 아닌 경우만 처리
        if birthday_str:
            try:
                birthday = datetime.strptime(birthday_str, "%Y%m%d")  # 8자리 포맷
                heap.insert((birthday, name))
            except ValueError:
                # 포맷 오류가 있는 경우 무시
                print(f"⚠ 잘못된 생일 포맷 제외됨: {name} - {birthday_str}")


print("생일이 늦은 친구 Top 10:") # 3. 생일이 늦은 친구 10명 출력
for i in range(10):
    if not heap.isEmpty():
        bday, name = heap.deleteMax()
        print(f"{i+1}. {name} - {bday.strftime('%Y-%m-%d')}")
    else:
        break
```

생일이 늦은 친구 Top 10:
1. 신수민 - 2005-12-30
2. 이서영 - 2005-12-25
3. 강민주 - 2005-12-14
4. 김민경 - 2005-12-02
5. 이서영 - 2005-11-12
6. 배시은 - 2005-11-02
7. 김여원 - 2005-10-31
8. 이서진 - 2005-10-28
9. 서홍빈 - 2005-10-24
10. 김예빈 - 2005-10-19

## 4. list

## 5. 8장 우선 순위 큐 연습문제
### 5-1. 
답: 아니다.
최대 힙에서는 부모 노드 ≥ 자식 노드 관계가 유지된다.
하지만 같은 깊이에 있는 노드들끼리 크기의 비교는 보장되지 않는다.
→ 따라서 더 깊은 노드가 루트보다 작을 수 있지만, 형제 노드보다는 클 수도 있다.

### 5-2. 
답: 아니다.
A[0]은 항상 최대지만, A[n-1]은 단지 리프 노드 중 하나일 뿐이다.
→ 따라서 항상 가장 작다고 보장할 수는 없다.

### 5-3.
답: 정확하게는 리프 노드 개수, 즉 ⌊n/2⌋ 개이다.
buildHeap()은 마지막 부모 노드부터 루트까지 거꾸로 올라가며 heapify를 한다.
→ 따라서 리프 노드들은 heapify할 필요가 없다.

### 5-4.
답: O(n)
buildHeap()의 전체 시간 복잡도는 → O(n)이다.

### 5-5. 
답: 그렇다.
힙은 루트 원소만 삭제하는 것이 기본 연산이다.
다른 위치의 원소 삭제는 해당 원소를 루트로 끌어올리거나 별도의 방식으로 처리해야 하므로 복잡한 작업이 된다.

### 5-6. 
답: 힙은 만들어지지만, 효율이 훨씬 떨어진다.
위에서부터 스며내리는 경우, 하위 트리에 불필요하게 많은 heapify 연산이 발생하므로
→ 시간복잡도가 증가하게 된다.
→ 따라서 기존 방식(아래에서 위로)이 더 효율적이다.

### 5-7. 
답:
값이 증가한 경우, 힙 특성(최대 힙이면 부모 ≥ 자식)을 유지하기 위해
→ 부모 노드 방향으로 스며올리는(percolate up / bubble up) 연산을 수행해야 한다.
→ 이 연산은 힙의 높이인 log n 만큼 수행되므로 시간복잡도는 O(log n)


## LeetCode 703.Kth Largest Element in Stream
```python
import heapq

class KthLargest:

    def __init__(self, k: int, nums: list[int]):
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)
        while len(self.heap) > k:
            heapq.heappop(self.heap)

    def add(self, val: int) -> int:
        heapq.heappush(self.heap, val)
        if len(self.heap) > self.k:
            heapq.heappop(self.heap)
        return self.heap[0]

```

# assignment05

## 3. heap
<pre>
<code>
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
</code>
</pre>

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

## 8장 우선 순위 큐 연습문제

## LeetCode 703.Kth Largest Element in Stream

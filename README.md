# assignment05
자료구조 과제5

## 3. 생일 데이터를 교재 코드(heap.py)를 이용해 힙으로 저장하고, 생일이 느린 순서로 10명의 친구를 출력하는 코드를 작성한다. 실행 결과가 셀에 나타나야 한다. 
'''
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
'''

print("🎂 생일이 늦은 친구 Top 10:") # 3. 생일이 늦은 친구 10명 출력
for i in range(10):
    if not heap.isEmpty():
        bday, name = heap.deleteMax()
        print(f"{i+1}. {name} - {bday.strftime('%Y-%m-%d')}")
    else:
        break

---
layout: post
title:  "Implementation"
date:   2022-01-26
categories: jekyll update
---
# Implementation(구현)
> 머릿속에 있는 알고리즘을 정확하고 빠르게 프로그램으로 작성하기

---
+ 유형
  - 완전 탐색: 모든 경우의 수를 주저 없이 다 계산하는 해결 방법
  - 시뮬레이션: 문제에서 제시한 알고리즘을 한 단계씩 차례대로 직접 수행해야 하는 문제 유형

+ 채점 환경
  - Pypy3는 파이썬3의 문법을 그대로 지원하며, 대부분 파이썬3보다 실행 속도가 더 빠르다. 코딩 테스트 환경이 파이썬3만 지원하는지, 혹은 Pypy3도 지원하는지 확인하고 만약 Pypy3도 지원한다면 이를 이용하도록 하자.

### 예제 1) 상하좌우
 　여행가 A는 N x N 크기의 정사각형 공간 위에 서 있다. 이 공간은 1 x 1 크기의 정사각형으로 나누어져 있다. 가장 왼쪽 위 좌표는 (1, 1)이며, 가장 오른쪽 아래 좌표는 (N, N)에 해당한다. 시작 좌표는 항상 (1, 1)이다. 이때, N x N 크기의 정사각형 공간을 벗어나는 움직임은 무시된다.

```python
n=int(input())
data=list(map(str, input().split()))

x,y=1,1 # 시작좌표는 (1,1)

for i in data:
    if i=="U" and x!=1:
        x-=1
    elif i=="D" and x!=n:
        x+=1
    elif i=="L" and y!=1:
        y-=1
    elif i=="R" and y!=n:
        y+=1
print(x,y)
```
 연산 횟수는 이동 횟수에 비례하게 된다. 예를 들어 이동 횟수가 N번인 경우 <u>시간 복잡도는 O(N)이다.</u> 이러한 문제는 일련의 명령에 따라서 개체를 차례대로 이동시킨다는 점에서 시뮬레이션 유형으로 분류되며 구현이 중요한 대표적인 문제 유형이다.

### 예제 2) 시각
  　정수 N이 입력되면 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 구하는 프로그램을 작성하시오. (0 <= N <= 23)
```python
n=int(input())
count=0
for i in range(n+1):
    for j in range(60):
        for h in range(60):
            if '3' in str(i)+str(j)+str(h): # 3이 하나라도 포함되는지
                count+=1
print(count)
```
이러한 유형은 완전 탐색 유형으로 분류되기도 한다. 완전 탐색 알고리즘은 가능한 경우의 수를 모두 검사해보는 탐색 방법이다. 완전 탐색 문제 또한 구현이 중요한 대표적인 문제 유형인데, 일반적으로 완전 탐색 알고리즘은 <u>비효율적인 시간 복잡도를 가지고 있으므로 데이터 개수가 큰 경우에 정상적으로 동작하지 않을 수 있다.</u>

### 예제 3) 왕실의 나이트
　왕실 정원은 8 x 8 좌표 평면이다. 나이트는 특정한 위치에서 다음과 같은 2가지 경우로 이동할 수 있다.
1. 수평으로 두 칸 이동한 뒤에 수직으로 한 칸 이동하기
2. 수직으로 두 칸 이동한 뒤에 수평으로 한 칸 이동하기

```python
input_data=input()
row=int(input_data[1])
column=int(ord(input_data[0]))-int(ord('a'))+1

steps=[(-2,-1),(-1,-2),(1,-2),(2,-1),(2,1),(1,2),(-1,2),(-2,1)]

result=0
for step in steps:
    # 이동하고자 하는 위치 확안
    next_row=row+step[0]
    next_column=column+step[1]
    # 해당 위치로 이동이 가능하다면 카운트 증가
    if next_row>=1 and next_row<=8 and next_column>=1 and next_column<=8:
        result+=1
print(result)
```

```python
# 내가 짠 코드
y,x=map(str,input())

row=['1','2','3','4','5','6','7','8']
col=['a','b','c','d','e','f','g','h']
count=0

for i in range(len(row)):
    if x == row[i]:
        x=i
for i in range(len(col)):
    if y == col[i]:
        y=i

if (y+2)<8: # 오른쪽 수평
    if (x+1)<8: # 아래로 한칸
        count+=1
    if (x-1)>=0: # 위로 한칸
        count+=1
if (y-2)>=0: # 왼쪽 수평
    if (x+1)<8: # 아래로 한칸
        count+=1
    if (x-1)>=0: # 위로 한칸
        count+=1

if (x+2)<8: # 아래 수평
    if (y+1)<8: # 오른쪽으로 한칸
        count+=1
    if (y-1)>=0: # 왼쪽으로 한칸
        count+=1
if (x-2)>=0: # 위로 수평
    if (y+1)<8: # 아래로 한칸
        count+=1
    if (y-1)>=0: # 왼쪽으로 한칸
        count+=1
print(count)

```


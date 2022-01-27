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
### 예제 4) 게임 개발
1. 현재 위치에서 현재 방향을 기준으로 왼쪽 방향(반시계 방향으로 90도 회전한 방향)부터 차례대로 갈 곳을 정한다.
2. 캐릭터의 바로 왼쪽 방향에 아직 가보지 않은 칸이 존재한다면, 왼쪽 방향으로 회전한 다음 왼쪽으로 한 칸을 전진핟다. 왼쪽 방향에 가보지 않은 칸이 없다면, 왼쪽 방향으로 회전만 수행하고 1단계로 돌아간다.
3. 만약 네 방향 모두 이미 가본 칸이거나 바다로 되어 있는 칸인 경우에는, 바라보는 방향을 유지한 채로 한 칸 뒤로 가고 1단계로 돌아간다. 단, 이때 뒤쪽 방향이 바다인 칸이라 뒤로 갈 수 없는 경우에는 움직임을 멈춘다.

메뉴얼에 따라 캐릭터를 이동시킨 뒤에, 캐릭터가 방문한 칸의 수를 출력하는 프로그램을 만드시오.

```python
n,m=map(int,input().split())

#컴프리헨션 문법을 사용해 2차원 리스트를 초기화
d=[[0]*m for _ in range(n)]

x,y,direction=map(int,input().split())
d[x][y]=1

array=[]
for i in range(n):
    array.append(list(map(int,input().split())))
# 북, 동, 남, 서 방향 정의
dx=[-1,0,1,0]
dy=[0,1,0,-1]

def turn_left():
    global direction
    direction-=1
    if direction==-1:
        direction=3

count=1
turn_time=0
while True:
    turn_left()
    nx=x+dx[direction]
    ny=y+dy[direction]
    # 회전한 이후 정면에 가보지 않은 칸이 존재하는 경우 이동
    if d[nx][ny]==0 and array[nx][ny]==0:
        d[nx][ny]=1
        x=nx
        y=ny
        count+=1
        turn_time=0
        continue
    # 회전한 이후 정면에 가보지 않은 칸이 없거나 바다인 경우
    else:
        turn_time+=1
    # 네 방향 모두 갈 수 없는 경우
    if turn_time==4:
        nx=x-dx[direction]
        ny=y-dy[direction]
        # 뒤로 갈 수 있다면 이동하기
        if array[nx][ny]==0:
            x=nx
            y=ny
        # 뒤가 바다로 막혀있는 경우
        else:
            break
        turn_time=0
print(count)
```
리스트 컴프리헨션 문법을 사용해 2차원 리스트를 초기화했다. 파이썬에서 2차원 리스트를 선언할 때는 컴프리헨션을 이용하는 것이 효율적이다.


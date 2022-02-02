---
layout: post
title:  "Shortest Path"
date:   2022-02-02
categories: jekyll update
---
# Shortest Path
> 특정 지점까지 가장 빠르게 도달하는 방법을 찾는 알고리즘

---
+ 다익스트라 최단 경로 알고리즘
  1. 출발 노드를 설정한다.
  2. 최단 거리 테이블을 초기화한다.
  3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
  4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.
  5. 위 고정에서 3, 4번을 반복한다.
  - 한 단계당 하나의 노드에 대한 최단 거리를 확실히 찾는다.
  - 입력값의 범위가 충분히 크면, 우선순위 큐를 이용하여 다익스트라 알고리즈을 작성해야 한다.

+ 플로이드 워셜 알고리즘
  - 모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우에 사용할 수 있는 알고리즘이다.

### 예제 1) 미래 도시
　N=5, X=4, K=5 이고 회사 간 도로가 7개면서 각 도로가 연결되어 있다.
이때 A는 1번부터 시작해서 K번을 거쳐 X번을 방문해야한다. 이때 최소 이동 시간을 구하라. (이동 거리는 모두 1)

```python
dn, m=map(int,input().split()) # 회사의 개수 N, 경로의 개수 M
INF=int(1e9)
graph=[[INF]*(n+1) for _ in range(n+1)]


for i in range(m):
    a,b=map(int,input().split())
    graph[a][b]=1
    graph[b][a]=1

x,k=map(int,input().split())

for i in range(1,n+1):
    for j in range(1,n+1):
        if i==j:
            graph[i][j]=0

for i in range(1,n+1):
    for j in range(1,n+1):
        for h in range(1,n+1):
            graph[j][h]=min(graph[j][h],graph[j][i]+graph[i][h])
distance=graph[1][x]+graph[x][k]
if distance>=INF:
    print(-1)
else:
    print(distance)
```
최단 거리는 <u>1번 노드에서 X까지의 최단 거리 + X에서 K까지의 최단 거리</u>이다.

### 예제 2) 전보
　도시 C에서 보낸 메시지를 받게 되는 도시의 개수와 도시들이 모두 메시지를 받는 데까지 걸리는 시간은 얼마인지 계산하는 프로그램을 작성하시오.
```python
import sys
import heapq
input=sys.stdin.readline
n, m , c=map(int,input().split()) # 도시의 개수 N, 통로의 개수 M, 메시지를 보내고자 하는 도시 C
INF=int(1e9)
graph=[[] for _ in range(n+1)]

for i in range(m):
    a,b,d=map(int,input().split())
    graph[a].append((b,d))

distance=[INF]*(n+1)

def dijkstra(start):
    q=[]

    heapq.heappush(q,(0,start))
    distance[start]=0

    while q:
        dist, now=heapq.heappop(q) # 최단거리 pop
        if distance[now]<dist:
            continue
        for i in graph[now]:
            cost=dist+i[1]
            if cost<distance[i[0]]:
                distance[i[0]]=cost
                heapq.heappush(q,(cost,i[0]))

dijkstra(c)

count=0
max_distance=0
for i in range(1,n+1):
    if not distance[i]==INF:
        max_distance=max(distance[i],max_distance)
        if not distance[i]==0:
            count+=1
print(count,max_distance)
```



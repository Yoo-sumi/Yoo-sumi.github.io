---
layout: post
title:  "Graph Theory"
date:   2022-02-03
categories: jekyll update
---
# Graph Theory
> 코딩 테스트에서 자주 등장하는 기타 그래프 이론 공부하기

---
+ 서로소 집합
  - union-find 자료구조라고 불리기도 한다.
  1. union 연산을 확인하며, 서로 연결된 두 노드 A, B를 확인한다.
    1. A와 B의 루트 노드 A', B'를 각각 찾는다.
    2. A'를 B'의 부모 노드를 설정한다(B'가 A'를 가리키도록 한다).
  2. 모든 union 연산을 처리할 때까지 1번 과정을 반복한다.
  3. find 함수는 경로 압축 기법을 적용하여 시간 복잡도를 개선시킬 수 있다.
  <br>
+ 서로소 집합을 활용한 사이클 판별
    1. 각 간선을 확인하며 두 노드의 루트 노드를 확인한다.
      - 루트 노드가 서로 다르다면 두 노드에 대하여 union 연산을 수행한다.
      - 루트 노드가 서로 같다면 사이클이 발생한 것이다.
    2. 그래프에 포함되어 있는 모든 간선에 대하여 1번 과정을 반복한다.

+ 신장 트리
   - 하나의 그래프가 있을 때 모든 노드를 포함하면서 사이클이 존재하지 않는 부분 그래프를 의미한다.
+ 크루스칼 알고리즘(최소 신장 트리 알고리즘)
   - 신장 트리 중에서 최소 비용으로 만들 수 있는 신장 트리를 찾는 알고리즘
   - 간선의 개수가 E개일 때, O(ElogE)의 시간 복잡도를 가진다.
   - 크루스칼 내부에서 사용되는 서로소 집합 알고리즘의 시간 복잡도는 정렬 알고리즘의 시간 복잡도보다 작으므로 무시한다.
   1. 간선 데이터를 비용에 따라 오름차순으로 정렬한다.
   2. 간선을 하나씩 확인하며 현재의 간선이 사이클을 발생시키는지 확인한다.
    - 사이클이 발생하지 않는 경우 최소 신장 트리에 포함시킨다.
    - 사이클이 발생하는 경우 최소 신장 트리에 포함시키지 않는다.
   3. 모든 간선에 대하여 2번의 과정을 반복한다.

+ 위상 정렬
    - 방향 그래프의 모든 노드를 방향성에 거스르지 않도록 순서대로 나열하는 것이다.
    - 시간 복잡도는 O(V+E)이다. (노드의 개수 V, 간선의 개수 E)
    1. 진입차수가 0인 노드를 큐에 넣는다.
    2. 큐가 빌 때까지 다음의 과정을 반복한다.
        - 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.
        - 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.

### 예제 1) 팀 결성
　학생들에게 0번부터 N번까지의 번호를 부여했다. 총 N+1팀이 존재한다. 이때 선샌님은 '팀 합치기' 연산과 '같은 팀 여부 확인' 연산을 사용할 수 있다. 선생님이 M개의 연산을 수행할 수 있을 때, '같은 팀 여부 확인' 연산에 대한 연산 결과를 출력하는 프로그램을 작성하시오. (팀 합치기 0, 같은 팀 여부 확인 1)

```python
def find_parent(parent,x):
    if parent[x]!=x:
        return find_parent(parent,parent[x])
    return parent[x]

def union(parent,b,c):
    e=find_parent(parent,b)
    f=find_parent(parent,c)
    if e<f:
        parent[f]=e
    else:
        parent[e]=f

n, m=map(int, input().split())

parent=[0]*(n+1)
for i in range(n):
    parent[i]=i


result=[]
for i in range(m):
    a,b,c=map(int,input().split())
    if a==0:
        union(parent,b,c)
    else:
        if find_parent(parent,b)==find_parent(parent,c):
            result.append("YES")
        else:
            result.append("NO")
for i in result:
    print(i)
```

### 예제 2) 도시 분할 계획
　마을은 N개의 집과 그 집들을 연결하는 M개의 길로 이루어져 있다. 길은 어느 방향으로든지 다닐 수 있는 편리한 길이다. 그리고 길마다 길을 유지하는데 드는 유지비가 있다. 마을을 2개의 분리된 마을로 분할할 계획을 세우고 있다. 각 분리된 마을 안에 있는 임의의 두 집 사이에 경로가 항상 존재해야 한다. 마을에는 집이 하나 이상 있어야 한다. 일단 분리된 두 마을 사이에 있는 길들은 필요가 없으므로 없앨 수 있다. 위 조건을 만족하도록 길들을 모두 없애고 나머지 길의 유지비의 합을 최소로 하고 싶다. 이것을 구하는 프로그램을 작성하시오.
```python
def find_parent(parent,x):
    if parent[x]!=x:
        return find_parent(parent,parent[x])
    return parent[x]
def union(parent,a,b):
    a=find_parent(parent,a)
    b=find_parent(parent,b)
    if a<b:
        parent[b]=a
    else:
        parent[a]=b

n, m=map(int,input().split())

parent=[0]*(n+1)
for i in range(1,n+1):
    parent[i]=i

edges=[]
for i in range(m):
    a,b,c=map(int,input().split())
    edges.append((c,a,b))
edges.sort()
result=0
last=0
for i in range(m):
    cost,a,b=edges[i]
    if find_parent(parent,a)!=find_parent(parent,b):
        union(parent,a,b)
        result+=cost
        last=cost
print(result-last)
```
크루스칼 알고리즘으로 최소 신장 트리를 찾은 뒤에 최소 신장 트리를 구성하는 간선 중에서 가장 비용이 큰 간선을 제거하는 것이다.



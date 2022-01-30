---
layout: post
title:  "Binary Search"
date:   2022-01-30
categories: jekyll update
---
# Binary Search
> 탐색 범위를 반으로 좁혀가며 빠르게 탐색하는 알고리즘

---
+ 순차 탐색
  - 리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법이다.
  - 시간 복잡도는 O(N)이다.

+ 이진 탐색
  - 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는 게 이진 탐색 과정이다.
  - 시간 복잡도는 O(logN)이다.
+ 빠르게 입력받기
  - 입력 데이터가 개수가 많은 문제에 input() 함수를 사용하면 동작 속도가 느려서 시간 초과로 오답 판정을 받을 수 있다.
  - 이처럼 입력 데이터가 많은 문제는 sys 라이브러리의 readline() 함수를 이용하면 시간 초과를 피할 수 있다.

  ```python
  import sys
  #하나의 문자열 데이터 입력받기
  input_data=sys.stdin.readline().rstrip()

  print(input_data)
  ```

### 예제 1) 부품 찾기
　손님이 요청한 부품 번호의 순서대로 부품을 확인해 부품이 있으면 yes를, 없으면 no를 출력한다.

```python
def binary_search(array, target, start, end):
    if start>end:
        return None
    mid=(start+end)//2
    if target==array[mid]:
        return mid
    elif array[mid]>target:
        return binary_search(array,target,start,mid-1)
    else:
        return binary_search(array,target,mid+1,end)


n=int(input())
data=list(map(int, input().split()))
data.sort()

m=int(input())
input_data=list(map(int, input().split()))
input_data.sort()

for i in range(m):
    result=binary_search(data,input_data[i],0,n-1)

    if result==None:
        print("No",end=" ")
    else:
        print("yes", end=" ")
```
### 예제 2) 떡볶이 떡 만들기
　손님이 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램을 작성하시오.
```python
n, m =map(int, input().split())
array=list(map(int,input().split()))
array.sort()
start=0
end=max(array)
result=0
while start<=end:
    mid=(start+end)//2
    sum=0
    for i in range(n):
        if array[i]>mid:
            sum+=array[i]-mid
    if sum<m:
        end=mid-1
    else:
        result=mid
        start=mid+1
print(result)
```
전형적인 이진 탐색 문제이다. 파라메트릭 서치 유형의 문제이다. <u>파라메트릭 서치는 최적화 문제를 결정 문제로 바꾸어 해결하는 기법이다.<u> 원하는 조건을 만족하는 가장 알맞은 값을 찾는 문제에 주로 파라메트릭 서치를 사용한다.



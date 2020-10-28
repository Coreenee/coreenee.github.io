---
layout: post
title: BAEKJOON 10021
---

## 문제

Due to a lack of rain, Farmer John wants to build an irrigation system to send water between his N fields (1 <= N <= 2000).

Each field i is described by a distinct point (xi, yi) in the 2D plane, with 0 <= xi, yi <= 1000. The cost of building a water pipe between two fields i and j is equal to the squared Euclidean distance between them:

(xi - xj)^2 + (yi - yj)^2

FJ would like to build a minimum-cost system of pipes so that all of his fields are linked together -- so that water in any field can follow a sequence of pipes to reach any other field.

Unfortunately, the contractor who is helping FJ install his irrigation system refuses to install any pipe unless its cost (squared Euclidean length) is at least C (1 <= C <= 1,000,000).

Please help FJ compute the minimum amount he will need pay to connect all his fields with a network of pipes.

## 입력

- Line 1: The integers N and C.
- Lines 2..1+N: Line i+1 contains the integers xi and yi.

## 출력

- Line 1: The minimum cost of a network of pipes connecting the fields, or -1 if no such network can be built.



---

##  문제 해설

![29](https://user-images.githubusercontent.com/37113547/97448764-55201a00-1974-11eb-93c7-b30f1f43fd16.jpeg)



## Code

```python
#이 코드는 시간초과 코드이다. 같은 코드를 C++로 구현했을 때는 제시간에 통과를 했다.
#아마도 언어차이가 아닌지,,,
from collections import defaultdict, deque
import heapq
def find(node_num, uf_list):
    if uf_list[node_num] < 0: return node_num
    else:
        uf_list[node_num] = find(uf_list[node_num], uf_list)
        return uf_list[node_num]

def merge(n1, n2, uf_list):
    n1_p = find(n1, uf_list)
    n2_p = find(n2, uf_list)
    if n1_p == n2_p:
        return False
    else:
        uf_list[n1] = n2
        return True

def get_distance(pos1, pos2):
    x, y = pos1 
    x1, y1 = pos2
    return (x-x1)**2 + (y-y1)**2



N, C = map(int, input().split())
node_list = []
tmp_idx = 0

for _ in range(N):
    x, y = map(int, input().split())
    node_list.append((tmp_idx, (x,y)))
    tmp_idx += 1

cand_list = []
for i in range(N-1):
    for j in range(i+1, N):
        dist = get_distance(node_list[i][1], node_list[j][1])
        if dist >= C:
            heapq.heappush(cand_list, (dist, node_list[i][0], node_list[j][0]))

if len(cand_list) < N-1:
    print(-1)
else:
    count = 0
    dist_sum = 0
    uf_list = [-1] * N
    while cand_list:
        dist, n1, n2 = heapq.heappop(cand_list)
        if merge(n1, n2, uf_list):
            dist_sum += dist
            count += 1

    if count == N-1: print(dist_sum)
    else: print(-1)
```


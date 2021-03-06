---
title: "[그래프 탐색 알고리즘] DFS와 BFS에 대해 알아보자"
date: 2020-12-31 +0800
categories: [알고리즘]
tags: [graph, dfs, bfs]

---

# DFS와 BFS에 대해 알아보자.

> DFS는 Deep First Search, BFS는 Breadth First Search로 그래프를 탐색하는 알고리즘 중 가장 대표적인 그래프 탐색 알고리즘이다.<br>
> DFS, BFS의 개념을 알고 파이썬으로 어떻게 구현하는지에 대해 알아보자.

## 그래프(Graph)

그래프를 표현하는 방법에는 대표적으로 두 가지가 있다. <br>
하나는 `인접 행렬로 표현하는 경우`, 다른 하나는 `인접 리스트로 표현하는 경우`이다.<br>
나의 경우에는 인접리스트로 그래프를 표현하는 방법을 주로 사용한다. <br>
아래의 코드처럼 간선이 주어지면 `인접리스트`로 그래프를 표현할 수 있다.

### 인접리스트로 그래프 표현하기
```python
n = 노드의 갯수
graph = list()
for i in range(n):
    graph.append([])
edge = [[0, 1], [1, 2], [1, 3], [2, 4]]

for [v, dst] in edge:
    graph[v].append(dst)
# graph[0] = [1]
# graph[1] = [2, 3]
# graph[2] = [4]
```

위 코드와 다르게 각 노드를 숫자가 아닌 문자로 표현하고 싶다면 인접리스트 대신 파이썬의 딕셔너리를 사용하면 될 것 같다.

아래는 내가 그래프를 나타내고 탐색알고리즘을 돌릴 때 `visited`라는 변수를 통해 각 노드 방문 유무를 확인할 때 쓰는 방법이다.

```python
# 1번째 방법
visited = []
# 노드 v를 방문을 할 때 마다
visited.append(v)

# 2번째 방법
n = 노드의 갯수
visited = [False] * n
# 노드 v를 방문 할 때 마다
visited[v] = True
```

## DFS(Deep First Search)

**DFS**는 깊이우선탐색으로 그래프를 탐색하는 데에 있어서 가능한 깊은곳 까지 탐색하고 더이상 탐색할 노드가 없으면 이전 노드로 돌아가면서 순회하는 탐색알고리즘이다.<br>
**DFS**는 `스택`을 활용하여 간단하게 구현할 수 있다.

### Stack으로 DFS 구현하기
```python
def dfs(graph, start):
    visited = []
    stack = [start]
    
    while q:
        node = stack.pop()
        if node not in visited:
            visited.append(node)
            stack.extend(graph[node])
```

## BFS(Breadth First Search)

**BFS**는 너비우선탐색으로 그래프를 탐색 할 때 현재 노드에 인접한 모든 노드를 우선적으로 탐색하고 다음 노드로 넘어가며 그래프를 순회하는 탐색알고리즘이다.<br>
**BFS**는 `큐`를 활용하여 간단하게 구현할 수 있다.

### Queue로 BFS 구현하기
```python
visited = [False] * (노드의 갯수)
def bfs(graph, start, visited):
    q = [start]
    
    while q:
        node = q.pop(0)
        if not visited[node]:
            visited[node] = True
            q.extend(graph[node])
```

특정 노드 v로 부터 너비우선 탐색을 하였을 때 각 노드의 최단 깊이를 알고싶으면 이런식으로도 구현할 수 있다.

### BFS로 노드의 최단 깊이 구하기
```python
global d # 각 노드의 깊이를 담은 리스트
d = [0] * (노드의 갯수)

def bfs(v, l, visited):
    global d
    depth = 0
    q = [(v, depth)]
    
    while(q):
        v, depth = q.pop(0)
        if not visited[v]:
            d[v] = depth
            visited[v] = True
            depth += 1
            for i in l[v]:
                q.append([i, depth])

print(d)
```

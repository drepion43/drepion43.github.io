---
title:  "DFS 응용"
categories: Coding
tag: [Coding, Algorithm, Theory, Python]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
---

## 조합

### Itertools

파이썬의 Itertools 사용시   
```python
import itertools
case = ['a', 'b', 'c', 'd', 'e']
print(list(itertools.combinations(case, 3)))
#[('a', 'b', 'c'), ('a', 'b', 'd'), ('a', 'b', 'e'), ('a', 'c', 'd'), ('a', 'c', 'e'), ('a', 'd', 'e'), ('b', 'c', 'd'), ('b', 'c', 'e'), ('b', 'd', 'e'), ('c', 'd', 'e')]
```

편하게 조합을 이용 할 수 있음

### DFS

Backtracking을 이용한 조합 구현   
```python
case = ['a', 'b', 'c', 'd', 'e']
n = len(case)
visited = [False] * n
choices = 3
answer = []
def dfs(idx, arr, depth):
    # nC3이 됬을 경우, 즉 모두 다 돌았을 대
    global answer
    if depth == choices:
        print(arr, end = ' ')
        answer.append(arr)
        return
    #
    for i in range(idx, n):
        if not visited[i]:
            arr.append(case[i])
            visited[i] = True
            dfs(i+1, arr, depth+1)
            arr.pop()
            visited[i] = False
dfs(0, [], 0)
# ['a', 'b', 'c'] ['a', 'b', 'd'] ['a', 'b', 'e'] ['a', 'c', 'd'] ['a', 'c', 'e'] ['a', 'd', 'e'] ['b', 'c', 'd'] ['b', 'c', 'e'] ['b', 'd', 'e'] ['c', 'd', 'e'] 
```


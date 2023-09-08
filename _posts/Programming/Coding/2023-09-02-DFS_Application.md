---
title:  "DFS 응용"
categories: Coding
tag: [Coding, Algorithm, Theory, Python, DFS]
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
from itertools import combinations
case = ['a', 'b', 'c', 'd', 'e']
print(list(combinations(case, 3)))
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
    # 방문 했던 것 이후로 확인
    for i in range(idx, n):
        if not visited[i]:
            arr.append(case[i])
            visited[i] = True
            # 방문 했던 것들은 지나치고 확인하게 되니 조합을 뜻함
            dfs(i+1, arr, depth+1)
            arr.pop()
            visited[i] = False
dfs(0, [], 0)
# ['a', 'b', 'c'] ['a', 'b', 'd'] ['a', 'b', 'e'] ['a', 'c', 'd'] ['a', 'c', 'e'] ['a', 'd', 'e'] ['b', 'c', 'd'] ['b', 'c', 'e'] ['b', 'd', 'e'] ['c', 'd', 'e'] 
```

```python
def do_something(arr):
    # 예시로는 출력 진행
    print(arr)
    pass

r = 3
case = [1, 2, 3, 4, 5]
n = len(case)
# backtracking
def dfs(depth, arr):
    # r개를 추출했을 때
    if len(arr) == r:
        # r개 추출했을 때 동작 진행
        do_something(arr)
        return
    # 모든 부분을 다 확인 했을 때(실패의 경우)
    elif depth == n:
        return

    dfs(depth+1, arr + [case[depth]])
    dfs(depth+1, arr)
dfs(0, [])
```

## 중복 조합

```python
def do_something(arr):
    # 예시로는 출력 진행
    print(arr)
    pass

r = 3
case = [1, 2, 3, 4, 5]
n = len(case)
visited = [False] * n
# backtracking
def dfs(depth, arr):
    # r개를 추출했을 때
    if len(arr) == r:
        # r개 추출했을 때 동작 진행
        do_something(arr)
        return
    # 모든 부분을 다 확인 했을 때(실패의 경우)
    elif depth == n:
        return
    for i in range(depth, n):
        arr.append(case[i])
        visited[i] = True
        dfs(i, arr)
        arr.pop()
        visited[i] = False

dfs(0, [])
```



## 순열

### Itertools

파이썬의 Itertools 사용시   

```python
from itertools import permutations
case = ['a', 'b', 'c', 'd', 'e']
print(list(permutations(case, 3)))
# [('a', 'b', 'c'), ('a', 'b', 'd'), ('a', 'b', 'e'), ('a', 'c', 'b'), ('a', 'c', 'd'), ('a', 'c', 'e'), ('a', 'd', 'b'), ('a', 'd', 'c'), ('a', 'd', 'e'), ('a', 'e', 'b'), ('a', 'e', 'c'), ('a', 'e', 'd'), ('b', 'a', 'c'), ('b', 'a', 'd'), ('b', 'a', 'e'), ('b', 'c', 'a'), ('b', 'c', 'd'), ('b', 'c', 'e'), ('b', 'd', 'a'), ('b', 'd', 'c'), ('b', 'd', 'e'), ('b', 'e', 'a'), ('b', 'e', 'c'), ('b', 'e', 'd'), ('c', 'a', 'b'), ('c', 'a', 'd'), ('c', 'a', 'e'), ('c', 'b', 'a'), ('c', 'b', 'd'), ('c', 'b', 'e'), ('c', 'd', 'a'), ('c', 'd', 'b'), ('c', 'd', 'e'), ('c', 'e', 'a'), ('c', 'e', 'b'), ('c', 'e', 'd'), ('d', 'a', 'b'), ('d', 'a', 'c'), ('d', 'a', 'e'), ('d', 'b', 'a'), ('d', 'b', 'c'), ('d', 'b', 'e'), ('d', 'c', 'a'), ('d', 'c', 'b'), ('d', 'c', 'e'), ('d', 'e', 'a'), ('d', 'e', 'b'), ('d', 'e', 'c'), ('e', 'a', 'b'), ('e', 'a', 'c'), ('e', 'a', 'd'), ('e', 'b', 'a'), ('e', 'b', 'c'), ('e', 'b', 'd'), ('e', 'c', 'a'), ('e', 'c', 'b'), ('e', 'c', 'd'), ('e', 'd', 'a'), ('e', 'd', 'b'), ('e', 'd', 'c')]

```

### DFS

```python
case = ['a', 'b', 'c', 'd', 'e']
n = len(case)
visited = [False] * n
choices = 3
def dfs(idx, arr):
    # nP3이 됬을 경우, 즉 모두 다 돌았을 대
    if len(arr) == choices:
        print(arr)
        return
    # 처음부터 확인
    for i in range(n):
        # 처음부터 방문 한적 없는 것들을 확인하게 되니 순열을 뜻함
        if not visited[i]:
            arr.append(case[i])
            visited[i] = True
            dfs(idx+1, arr)
            arr.pop()
            visited[i] = False

dfs(0, [])
```

## 중복 순열

```python
def do_something(arr):
    # 예시로는 출력 진행
    print(arr)
    pass

r = 3
case = [1, 2, 3, 4, 5]
n = len(case)
visited = [False] * n
# backtracking
def dfs(depth, arr):
    # r개를 추출했을 때
    if len(arr) == r:
        # r개 추출했을 때 동작 진행
        do_something(arr)
        return
    # 모든 부분을 다 확인 했을 때(실패의 경우)
    elif depth == n:
        return
    for i, value in enumerate(case):
        arr.append(value)
        visited[i] = True
        dfs(i, arr)
        arr.pop()
        visited[i] = False

dfs(0, [])
```

## 나선형 탐색

<https://www.acmicpc.net/problem/20057>

![img](https://upload.acmicpc.net/37e7aa13-0f2b-49d6-af68-e745537b1ea3/-/preview/)

방향 : 좌, 하, 우, 상 으로 탐색   
방향대로 2번 이동한 후 1칸씩 탐색 크기 칸을 증가 시킴   
ex) 좌로 1칸 이동, 하로 1칸이동, 우로 2칸 이동, 상으로 2칸 이동, 좌로 3칸 이동, 하로 3칸 이동, ...   

```python
# 보드 사이즈
n = 7
board = [[0] * n for _ in range(n)]
# 이동 방향 : 좌 하 우 상
dx = [0, 1, 0, -1]
dy = [-1, 0, 1, 0]

# 시작 지점 : 가운데 지점
start = int(n/2)
sx = sy = start
# 시작 방향 : 좌
dir = 0
# 탐색 번호
num = 0

def tornado(sx, sy, dir, num):
    # 이동 하는 거리 수
    move_distance = 1
    # 이동 횟수(2번 이동한 후 이동 거리 수 1 증가)
    move_count = 0
    while True:
        move_count += 1
        for i in range(move_distance):
            nx, ny = sx + dx[dir], sy + dy[dir]

            # 끝에 도착하는 경우 : (0, -1)
            if nx == 0 and ny == -1:
                return
            num += 1
            # 탐색 번호 지정
            board[nx][ny] = num
            # 좌표 변경
            sx, sy = nx, ny

        # 방향 전환
        dir = (dir + 1) % 4
        # 이동 거리 증가
        if move_count == 2:
            move_distance += 1
            move_count = 0
tornado(sx, sy, dir, num)
for row in board:
    print(row)
```

```ba
[48, 47, 46, 45, 44, 43, 42]
[25, 24, 23, 22, 21, 20, 41]
[26, 9, 8, 7, 6, 19, 40]
[27, 10, 1, 0, 5, 18, 39]
[28, 11, 2, 3, 4, 17, 38]
[29, 12, 13, 14, 15, 16, 37]
[30, 31, 32, 33, 34, 35, 36]
```

## 배열 회전

배열을 45도, 90도, 180도로 회전 시키기   
```py
# 보드 사이즈
n = 7
board = [[i * n + j for j in range(n)] for i in range(n)]
# 45도 회전
def rotate_45(board):
    # 45도 회전시킨 값들을 담을 배열
    new_board = [[0] * n for _ in range(n)]
    for x in range(n):
        for y in range(n):
            # (0,0)의 값은 (0,n-1)로, (n-1,n-1)은 (n-1, 0)으로 이동
            new_board[x][y] = board[n-1-y][x]
    return new_board
new_board = rotate_45(board)
for row in new_board:
    print(row)

def rotate_90(board):
    # 90도 회전시킨 값들을 담을 배열
    new_board = [[0] * n for _ in range(n)]
    for x in range(n):
        for y in range(n):
            # (0,0)의 값은 (n-1,n-1)로, (n-1,n-1)은 (0, 0)으로 이동
            new_board[x][y] = board[n-1-x][n-1-y]
    return new_board
print()
new_board = rotate_90(board)
for row in new_board:
    print(row)
```

```bas
원본
[0, 1, 2, 3, 4, 5, 6]
[7, 8, 9, 10, 11, 12, 13]
[14, 15, 16, 17, 18, 19, 20]
[21, 22, 23, 24, 25, 26, 27]
[28, 29, 30, 31, 32, 33, 34]
[35, 36, 37, 38, 39, 40, 41]
[42, 43, 44, 45, 46, 47, 48]

45도 회전
[42, 35, 28, 21, 14, 7, 0]
[43, 36, 29, 22, 15, 8, 1]
[44, 37, 30, 23, 16, 9, 2]
[45, 38, 31, 24, 17, 10, 3]
[46, 39, 32, 25, 18, 11, 4]
[47, 40, 33, 26, 19, 12, 5]
[48, 41, 34, 27, 20, 13, 6]

90도 회전
[48, 47, 46, 45, 44, 43, 42]
[41, 40, 39, 38, 37, 36, 35]
[34, 33, 32, 31, 30, 29, 28]
[27, 26, 25, 24, 23, 22, 21]
[20, 19, 18, 17, 16, 15, 14]
[13, 12, 11, 10, 9, 8, 7]
[6, 5, 4, 3, 2, 1, 0]
```


---
title:  "[코드트리]마법의 숲 탑색"
categories: Coding
tag: [Coding, Coding Test, Baekjoon, Python, Simulation, BFS]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
date: 2024-06-14
toc_sticky: true
---

## 마법의 숲 탐색

<https://www.codetree.ai/training-field/frequent-problems/problems/magical-forest-exploration/description?page=1&pageSize=20>

### 문제

### 입력


### 해결법
```python
from collections import deque
import copy
# 하로 내려감
# 만약 하가 막혀잇으면, 좌를 확인 후 좌가 모두 비어있으면 좌료 이동 후 하로 이동 -> 이 때 출구는 반시계방향으로 회전함
# 좌도 막혀잇다면, 우로 회전하며 내려감 -> 출구도 또한 시계방향으로 회전

# 출구가 다른 골렘과 인접해잇다면 다른 골렘으로 이동 가능
# 최종 목표 : 가능한 가장 남쪽으로 가야함 -> 정답은 행의 최종 누적 합
# 만약 골렘의 몸이 숲의 일부를 벗어난다면, 그 때는 답에 포함시키지 않음

# 숲 크기, 정령 수
r, c, k = map(int, input().split())

board = [[0] * c for _ in range(r)]

# 골렘
monsters = []
# 출구 방향
exit_dir = []
for _ in range(k):
    # 출발하는 열, 방향
    sc, sd = map(int, input().split())
    monsters.append([sc - 1, sd])
    exit_dir.append(sd)

# 방향 : 상 우 하 좌
dx = [1, 0, -1, 0]
dy = [0, 1, 0, -1]

# 현재 중심 좌표가 보드판위에 존재할 수 있는 좌표인지 확인
def is_range(_x, _y, _board):
    if 0 <= _x < r and 0 <= _y < c and _board[_x][_y] == 0:
        for i in range(4):
            nx, ny = _x + dx[i], _y + dy[i]
            if 0 > nx or nx >= r or 0 > ny or ny >= c or _board[nx][ny] > 0:
                return False
        return True
    else:
        return False

# 현재 골렘 중심을 기준으로 골렘 좌표 세팅
def setting_coord(_x, _y, _board):
    _board[_x][_y] = 1
    _board[_x-1][_y] = 1
    _board[_x+1][_y] = 1
    _board[_x][_y-1] = 1
    _board[_x][_y+1] = 1
    return _board

# 현재 골렘 중심을 기준으로 골렘 좌표 해제
def unsetting_coord(_x, _y, _board):
    _board[_x][_y] = 0
    _board[_x-1][_y] = 0
    _board[_x+1][_y] = 0
    _board[_x][_y-1] = 0
    _board[_x][_y+1] = 0
    return _board

#

# 아래로 갈 수 있는지 확인
def check_down(_x, _y, _board):
    tmp_board = copy.deepcopy(_board)
    tmp_board = unsetting_coord(_x, _y, tmp_board)
    print('clean', tmp_board)
    return is_range(_x + 1, _y, tmp_board)

# 좌로 갈 수 있는지 확인 -> 좌로 갈 때, 좌 확인 + 아래 확인
def check_left(_x, _y, _board):
    tmp_board = copy.deepcopy(_board)
    tmp_board = unsetting_coord(_x, _y, tmp_board)
    return is_range(_x , _y - 1, tmp_board)

# 우로 갈 수 있는지 확인 -> 우로 갈 때, 우 확인 + 아래 확인
def check_right(_x, _y, _board):
    tmp_board = copy.deepcopy(_board)
    tmp_board = unsetting_coord(_x, _y, tmp_board)
    return is_range(_x , _y + 1, tmp_board)

# 해당 방향으로 이동(현 중심좌표, 보드, 내려갈 방향)
def move(_x, _y, _board, _d):
    if is_range(_x, _y, _board):
        _board = unsetting_coord(_x, _y, _board)
    # 아래로 갈 경우, 아래로 이동
    if _d == 1:
        _board = setting_coord(_x+1, _y, _board)
        return _board, _x+1, _y
    # 좌료 갈 경우, 좌 + 아래로 이동
    elif _d == 2:
        _board = setting_coord(_x+1, _y-1, _board)
        return _board, _x+1, _y-1
    # 우로 갈 경우, 우 + 아래로 이동
    elif _d == 3:
        _board = setting_coord(_x+1, _y+1, _board)
        return _board, _x+1, _y+1


# 더 이상 못내려갈 때, 출구가 인접골렘과 연결되있다면 이동(출구 방향) -> 최대 깊이 return
def check_exit_adjac(_x, _y, _board, _ed):
    cx, cy = _x + dx[_ed], _y + dy[_ed]
    q = deque()
    visited = [[False] * c for _ in range(r)]
    q.append([cx, cy])
    # 출구 좌표 방문 처리
    visited[cx][cy] = True
    # 중심 좌표 방문 처리로 현재 골렘에서 이동하는 것 방지
    visited[_x][_y] = True
    #가장 큰 x좌표
    max_x = 0
    # BFS -> 출구쪽에서 연결된 갈 수 있는 곳 모든 곳을 돌아보며 최대 깊이 찾기
    while q:
        x, y = q.popleft()
        # 최대 깊어 얻기
        max_x = max(max_x, x)
        for i in range(4):
            nx, ny = _x + dx[i], _y + dy[i]
            if 0 <= nx < r and 0 <= ny < c and _board[nx][ny] > 0 and not visited[nx][ny]:
                visited[nx][ny] = True
                q.append([nx, ny])
    # 출구에서 인접한 경우가 없는경우 -> 출구 x좌표와 동일한 경우
    if max_x == cx:
        return _x + 1 + 1
    else:
        return max_x + 1




answer = 0
for idx, monster in enumerate(monsters):
    my, md = monster
    # 우선 중심좌표를 0으로 세팅
    mx = 0
    # 처음에는 일단 어디든지 내려갈 수 있는지 확인이 필요
    is_possible = False
    # 아래로 갈 수 있는지 확인 후 이동
    if check_down(mx, my, board):
        if is_range(mx + 1, my, board):
            is_possible = True
    # 좌료 갈 수 있는지 확인 및 좌로 이동 후 아래로도 갈 수 있는지 확인
    elif check_left(mx, my, board) and check_down(mx, my - 1, board):
        if is_range(mx + 1, my - 1, board):
            is_possible = True
    # 우료 갈 수 있는지 확인 및 우로 이동 후 아래로도 갈 수 있는지 확인
    elif check_right(mx, my, board) and check_down(mx, my + 1, board):
        if is_range(mx + 1, my + 1, board):
            is_possible = True

    if not is_possible:
        board = [[0] * c for _ in range(r)]
        continue
    print()
    # 내려가야하는 방향
    down_dir = 1
    while True:
        # 아래로 갈 수 있는지 확인 후 이동
        if check_down(mx, my, board):
            down_dir = 1
            board, mx, my = move(mx, my, board, down_dir)
        # 좌료 갈 수 있는지 확인 및 좌로 이동 후 아래로도 갈 수 있는지 확인
        elif check_left(mx, my, board) and check_down(mx, my-1, board):
            down_dir = 2
            board, mx, my = move(mx, my, board, down_dir)
            md = (md - 1) % 4
        # 우료 갈 수 있는지 확인 및 우로 이동 후 아래로도 갈 수 있는지 확인
        elif check_right(mx, my, board) and check_down(mx, my + 1, board):
            down_dir = 3
            board, mx, my = move(mx, my, board, down_dir)
            md = (md + 1) % 4
        else:
            break
        print(board)
        print(mx, my)
    # print(board, md)
    # tmp_answer = check_exit_adjac(mx, my, board, md)
```
---
title:  "[코드트리]메이즈러너"
categories: Coding
tag: [Coding, Coding Test, Baekjoon, Python, Simulation, DFS]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
date: 2024-04-08
toc_sticky: true
---

## 메이즈 러너

<https://www.codetree.ai/training-field/frequent-problems/problems/maze-runner/description?page=1&pageSize=20>

### 문제
M명의 참가자가 미로 탈출하기 게임에 참가하였습니다.

미로의 구성은 다음과 같습니다.

1. 미로는 N×N 크기의 격자입니다. 각 위치는 (r,c)의 형태로 표현되며, 아래로 갈수록 r이 증가, 오른쪽으로 갈수록 c가 증가합니다. 좌상단은 (1,1)입니다.
2. 미로의 각 칸은 다음 3가지 중 하나의 상태를 갖습니다.
- 빈 칸
    - 참가자가 이동 가능한 칸입니다.
- 벽
    - 참가자가 이동할 수 없는 칸입니다.
    - 1이상 9이하의 내구도를 갖고 있습니다.
    - 회전할 때, 내구도가 1씩 깎입니다.
    - 내구도가 0이 되면, 빈 칸으로 변경됩니다.
- 출구
    - 참가자가 해당 칸에 도달하면, 즉시 탈출합니다.
    - 1초마다 모든 참가자는 한 칸씩 움직입니다. 움직이는 조건은 다음과 같습니다.

두 위치 (x1,y1), (x2,y2)의 최단거리는 ∣x1−x2∣+∣y1−y2∣로 정의됩니다.
- 모든 참가자는 동시에 움직입니다.
- 상하좌우로 움직일 수 있으며, 벽이 없는 곳으로 이동할 수 있습니다.
- 움직인 칸은 현재 머물러 있던 칸보다 출구까지의 최단 거리가 가까워야 합니다.
- 움직일 수 있는 칸이 2개 이상이라면, 상하로 움직이는 것을 우선시합니다.
- 참가가가 움직일 수 없는 상황이라면, 움직이지 않습니다.
- 한 칸에 2명 이상의 참가자가 있을 수 있습니다.

모든 참가자가 이동을 끝냈으면, 다음 조건에 의해 미로가 회전합니다.

- 한 명 이상의 참가자와 출구를 포함한 가장 작은 정사각형을 잡습니다.
- 가장 작은 크기를 갖는 정사각형이 2개 이상이라면, 좌상단 r 좌표가 작은 것이 우선되고, 그래도 같으면 c 좌표가 작은 것이 우선됩니다.
- 선택된 정사각형은 시계방향으로 90도 회전하며, 회전된 벽은 내구도가 1씩 깎입니다.
K초 동안 위의 과정을 계속 반복됩니다. 만약 K초 전에 모든 참가자가 탈출에 성공한다면, 게임이 끝납니다. 게임이 끝났을 때, 모든 참가자들의 이동 거리 합과 출구 좌표를 출력하는 프로그램을 작성해보세요.
### 입력

첫 번째 줄에 N, M, K가 공백을 사이에 두고 주어집니다.

다음 N개의 줄에 걸쳐서 N×N 크기의 미로에 대한 정보가 주어집니다.

- 0이라면, 빈 칸을 의미합니다.
- 1이상 9이하의 값을 갖는다면, 벽을 의미하며, 해당 값은 내구도를 뜻합니다.
다음 M개의 줄에 걸쳐서 참가자의 좌표가 주어집니다. 모든 참가자는 초기에 빈 칸에만 존재합니다.

다음 줄에 출구의 좌표가 주어집니다. 출구는 빈 칸에만 주어집니다.

- N: 미로의 크기 (4≤N≤10)
- M: 참가자 수 (1≤M≤10)
- K: 게임 시간 (1≤K≤100)

### 해결법
해당 문제의 경우 우선 k번 만큼 입력 받은 (r,c,s)회전 연산에 대한 순서를 정해야합니다. 이 순서는 n!인 $nP_n$이므로 백트래킹을 이용한 순열을 통해 순서를 정했습니다.    
그 다음 배열을 돌리는데, 가운데 지점인 (r, c)는 그대로 둔체 그 바깥쪽을 시계방향으로 1칸씩 이동시키면 됩니다. 이 때, 좌측 상단과 우측 하단의 경우는 좌표값이 확 틀어지는 구간입니다. 좌측 상단은 해당 하단 위치의 x+1값으로 대체를 해주고 우측 하단은 x-1의 위치의 배열이 해당 위치를 차지하게됩니다. 그 외에는 우측으로 이동, 아래로 이동, 좌측으로 이동, 위로 이동이 일어나게됩니다. 
```python
from collections import deque
import copy

n, m, k = map(int, input().split())

board = []
for _ in range(n):
    data = list(map(int, input().split()))
    board.append(data)

players = []
for _ in range(m):
    px, py = map(int, input().split())
    players.append([px - 1, py - 1])
    board[px - 1][py - 1] = -1

# 출구 좌표
exit_x, exit_y = map(int, input().split())
board[exit_x - 1][exit_y - 1] = -2

# 상하 좌우 (상하가 우선)
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]


def erase_player(_board, _players, ex, ey):
    for x, y in _players:
        if x == ex and y == ey:
            continue
        _board[x][y] = 0
    return _board


def setting_player(_board, _players):
    for x, y in _players:
        _board[x][y] = -1
    return _board


# 시계방향 90도 부분 회전(현재 전체 보드판, 정사각형 가장 왼쪽상단 좌표, 정사각형 길이)
def rotate(sy, sx, length, _board, _players):
    tmp_board = copy.deepcopy(_board)
    update_player = copy.deepcopy(_players)
    # 정사각형을 시계방향으로 90도 회전
    for y in range(sy, sy + length):
        for x in range(sx, sx + length):
            # 1단계 : (0,0)으로 옮겨주는 변환을 진행함
            oy, ox = y - sy, x - sx
            # 2단계 : 90도 회전했을때의 좌표를 구함
            ry, rx = ox, length - oy - 1
            # 3단계 : 다시 (sy,sx)를 더해줌
            tmp_board[sy + ry][sx + rx] = _board[y][x]
            for idx, (px, py) in enumerate(_players):
                # sx sy : 0, 1, length : 2
                # x, y : (1, 2) -> ox, oy : (1, 1) -> rx, ry : (1, 0)
                if px == y and py == x:
                    update_player[idx] = [sy + ry, sx + rx]


    return tmp_board, update_player


# 벽 값 감소 및 출구 좌표 획득 및 플레이어 위치 탐색
def minus_wall(_board, sx, sy, s_length):
    ex, ey = -1, -1
    for x in range(n):
        for y in range(n):
            # 벽 감소
            if _board[x][y] > 0 and sx <= x < sx + s_length and sy <= y < sy + s_length:
                _board[x][y] -= 1
            # 출구 좌표
            elif _board[x][y] == -2:
                ex, ey = x, y
    return ex, ey


# 참가자와 출구까지 거리, or 사용좌 좌표
def get_dis(ex, ey, cx, cy):
    return abs(ex - cx) + abs(ey - cy), cx, cy


# 정사각형 좌측 상단 좌표 얻기
def get_square(ex, ey, _players):
    global n
    min_dis = []
    for x, y in _players:
        dis = max(abs(x - ex), abs(y - ey))
        min_dis.append([dis, x, y])
    # 거리가 가장 가까우며, x값이 작은것, y값이 작은것 순
    min_dis.sort(key=lambda x: (x[0], x[1], x[2]))
    # , 출구에서 가장가까운 사람 좌표
    square_lengths, _, _ = min_dis[0]
    # 정사각형 꼭지점 좌표 얻기
    # square_lengths = max(abs(tx - ex), abs(ty - ey))
    # print('choice person', square_lengths)

    ################ 사각형 고르기 다시 고민
    # (0,0)부터 정사각형 길이를 아니 출구와 가장 가까운 참가자를 포함하는 정사각형 좌상단 좌표 찾기
    is_find = False
    for x in range(n - square_lengths):
        if is_find:
            break
        for y in range(n - square_lengths):
            if is_find:
                break
            left_up_x, left_up_y = x, y
            right_down_x, right_down_y = x + square_lengths, y + square_lengths
            if left_up_x <= ex <= right_down_x and left_up_y <= ey <= right_down_y:
                for px, py in _players:
                    if left_up_x <= px <= right_down_x and left_up_y <= py <= right_down_y:
                        is_find = True
                        sx, sy = x, y

    return square_lengths + 1, sx, sy


# 참가자 이동 가능 확인 및 이동
def move(ex, ey, _board, _players):
    players_move = 0
    update_players = copy.deepcopy(_players)
    for idx, player in enumerate(_players):
        x, y = player
        # 현재 플레이어 위치에서의 거리
        cur_dis, _, _ = get_dis(ex, ey, x, y)
        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            # 보드판 내부이며 벽이 아닌 경우 이동 가능
            if 0 <= nx < n and 0 <= ny < n and (_board[nx][ny] == 0 or _board[nx][ny] == -1 or _board[nx][ny] == -2):
                # 현재 위치보다 이동할 위치의 거리가 더 작은지 확인
                after_dis, _, _ = get_dis(ex, ey, nx, ny)
                # 이동한 위치가 출구보다 가까우면 이동
                if after_dis <= cur_dis:
                    # # 출구에 도착한 경우
                    # if _board[nx][ny] == -2:
                    #     _board[x][y] = 0
                    # else:
                    #     _board[x][y] = 0
                    update_players[idx] = [nx, ny]
                    players_move += 1
                    break
    # 참가자들이 이동한 거리
    return players_move, _players, update_players


# 참가자 출구 도착인지 확
def arrive_player(ex, ey, _players):
    update_player = []
    for idx, player in enumerate(_players):
        x, y = player
        if x == ex and y == ey:
            continue
        else:
            update_player.append(player)
    return update_player


def start(ex, ey, k, _board, _players):
    global n
    total_dis = 0
    cur_ex, cur_ey = ex, ey
    while k > 0:
        # print(cur_ex+1, cur_ey+1)
        # 이동 확인 및 이동 거리 측정
        # for x in range(n):
        #     print(_board[x])
        # print('before move', _players)
        move_dis, before_players, _players = move(cur_ex, cur_ey, _board, _players)
        total_dis += move_dis
        # print('after move', _players)
        # print('move score', total_dis)
        # 이전 참가자들 좌표 삭제
        _board = erase_player(_board, before_players, cur_ex, cur_ey)
        # print('erase board')
        # for x in range(n):
        #     print(_board[x])
        # 탈출할 플에이어는 탈출
        _players = arrive_player(cur_ex, cur_ey, _players)
        # print('update player', _players)
        # 이동한 참가자들 좌표 설정
        _board = setting_player(_board, _players)
        if len(_players) == 0:
            break
        # print('after move')
        # for x in range(n):
        #     print(_board[x])

        # 정사각형 구하기
        s_lengths, sx, sy = get_square(cur_ex, cur_ey, _players)
        # 정사각형 회전
        _board, _players = rotate(sx, sy, s_lengths, _board, _players)
        # print('choice square', sx, sy, s_lengths)
        # print('after rotate player', _players)
        # 벽 감소 및 변경된 출구 좌표
        cur_ex, cur_ey = minus_wall(_board, sx, sy, s_lengths)
        # 플레이어가 모두 탈출했는지 확인
        if len(_players) == 0:
            break
        # print()

        k -= 1
    print(total_dis)
    print(cur_ex + 1, cur_ey + 1)


start(exit_x - 1, exit_y - 1, k, board, players)
```
---
title:  "[코드트리]싸움땅"
categories: Coding
tag: [Coding, Coding Test, CodeTree, Python, Simulation]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
date: 2024-09-25
toc_sticky: true
---

## 싸움땅

<https://www.codetree.ai/training-field/frequent-problems/problems/battle-ground/description?page=3&pageSize=5>

### 문제


### 입력


### 해결법
  
```python
from collections import deque

# 1. 본인의 방향대로 움직임(벽일경우 반대방향으로)
# 2-1. 이동한 방향의 총이 있을경우, 본인의 총과 비교해 더 높은 총을 획득(낮은 총들은 놔둠)
# 2-2. 플레이어와 만날시 본인 능력치 + 총 공격력 비교 후 더 큰 플레이어가 승리(합 능력치가 동일시 기본 능력치가 높은 플레이어가 승리), 이긴 플레이어는 그 차이만큼을 점수로 획득
# 2-2-2. 진 플레이어는 총을 그 칸에 놔두고 본인의 방향으로 다시 한칸 이동(이동칸에 다른 플레이어가 존재시 시계방향 90도 방향으로 회전 후 이동)
# 2-2-3. 이긴 플레이어는 진 플레이어가 놔둔 총과 모두 비교하여 가장 높은 공격력의 총으로 변경
# n번 플레이어가 모두 상의 행위 종료시 1개의 라운드 종료


# 보드판 크기, 플레이어 수, 라운드 수 
n, m, k = map(int, input().split())

# 플레이어의 위치를 담을 보드판
board = [[0] * n for _ in range(n)]
players = []
# 플레이어의 점수
scores = [0] * m
# 총 개수를 닮을 리스트 -> 보드판과 동일하지만, 한개의 셀에 여러개의 총을 담을 수 있게, 리스트로 관리 -> 리스트는 항상 내림차순으로 정렬
guns = []

for _ in range(n):
    data = list(map(int, input().split()))
    gun_num = []
    # 각 총의 공격력을 리스트로 담아 초기화
    for gun in data:
        if gun != 0:
            gun_num.append([gun])
        else:
            gun_num.append([])
    guns.append(gun_num)

# x, y, d, s -> d : 상 우 하 좌
# players -> 위치, 방향,
for i in range(m):
    x, y, d, s = map(int, input().split())
    players.append([x-1, y-1, d, s, 0])
    # 현재 플레이어가 있는 위치 보드판에 기록
    board[x-1][y-1] = i + 1

# 상 우 하 좌
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

# 총 바꾸기
def change_gun(sx, sy, cur_gun):
    if len(guns[sx][sy]) > 0:
        gun = guns[sx][sy][0]
        if cur_gun < gun:
            # 새로운 총 들기, 들고 있던 총은 내려놓기, 높은 순으로 총 재정렬
            guns[sx][sy].pop(0)
            guns[sx][sy].append(cur_gun)
            guns[sx][sy].sort(reverse=True)
            cur_gun = gun
    return cur_gun

# 사람 이동 -> 위치 이동 + 총 들기
def move(_x, _y, _d, _gun):
    sx, sy = _x + dx[_d], _y + dy[_d]
    # 벽 만났을 시 반대 방향으로 이동
    if (0 > sx or n <= sx) or (0 > sy or n <= sy):
        _d = (_d + 2) % 4
        sx, sy = _x + dx[_d], _y + dy[_d]
    return sx, sy, _d, _gun

# 이동한 방향에 플레이어가 존재하는지 확인
def match_players(_idx, _x, _y):
    for idx, (cx,cy, _, _, _) in enumerate(players):
        if _idx == idx:
            continue
        if _x == cx and _y == cy:
            # 이미 존재하는 플레이어 번호
            return idx
    return -1

# 이동한 방향에 플레이어가 존재시 게임 진행 -> 진 플레이어 번호 return
# _s, _d : 본인 공격력
def fight(_target_idx, _source_idx, _s, _gun):
    # 상대
    x, y, d, s, gun = players[_target_idx]
    target_player_score = s + gun
    # 본인
    source_player_score =_s + _gun
    # print('score', source_player_score, target_player_score)
    # 본인이 이길시
    if source_player_score > target_player_score:
        # 진 플레이어 번호 return
        return _target_idx, abs(target_player_score - source_player_score)
    # 점수가 동일시
    elif source_player_score == target_player_score:
        if _s > s:
            return _target_idx, abs(target_player_score - source_player_score)
        else:
            return _source_idx, abs(target_player_score - source_player_score)
    # 상대가 이길시
    else:
        return _source_idx,  abs(target_player_score - source_player_score)

# 진 플레이어의 이동
def lose_move(_idx, _x, _y, _d, _gun):
    sx, sy = _x + dx[_d], _y + dy[_d]
    # 이동한 방향에 플레이어가 존재시 or 벽 만났을 시 반대 방향으로 이동
    sd = _d
    if ((0 > sx or n <= sx) or (0 > sy or n <= sy)) or match_players(_idx, sx, sy) != -1:
        for i in range(4):
            nd = (_d + i) % 4
            nx, ny = _x + dx[nd], _y + dy[nd]
            if match_players(_idx, nx, ny) == -1 and (0 <= nx < n and 0 <= ny < n):
                sx, sy, sd = nx, ny, nd
                break
    return sx, sy, sd, _gun


# 시물레이션
for r in range(k):
    for num in range(m):
        x, y, d, s, gun = players[num]
        # 1. 이동
        nx, ny, d, gun = move(x, y, d, gun)
        # print(num, nx, ny, gun)
        # 2. 이동 위치에 플레이어 존재하는지 찾기
        target_idx = match_players(num, nx, ny)
        # print(target_idx)
        # 플레이어가 있을 시
        if target_idx != -1:
            tx, ty, td, ts, tg = players[target_idx]
            # 이동한 방향에 총이 있을시, 바꾸기
            # gun = change_gun(nx, ny, gun)
            # print('target vs source ', ts, tg, s, gun)
            loser_idx, score = fight(target_idx, num, s, gun)
            # 본인이 졌을시
            if loser_idx == num:
                # 이긴 플레이어가 차만큼 점수 획득
                scores[target_idx] += score
                # 총 내려놓기
                guns[nx][ny].append(gun)
                guns[nx][ny].sort(reverse=True)
                gun = 0
                # 이동
                # 이긴 플레이어 이동
                # 이긴 사람은 총 고르기
                tg = change_gun(tx, ty, tg)
                players[target_idx] = [tx, ty, td, ts, tg]
                # 진 플레이어 이동
                nx, ny, d, gun = lose_move(num, nx, ny, d, gun)
                # 총이 있다면 총 들기
                gun = change_gun(nx, ny, gun)
                # 위치 업데이트
                players[num] = [nx, ny, d, s, gun]
            # 본인이 이겼을 시
            else:
                # 이긴 플레이어가 차만큼 점수 획득
                scores[num] += score
                # 총 내려놓기
                guns[tx][ty].append(tg)
                guns[tx][ty].sort(reverse=True)
                tg = 0
                # 이동
                # 이긴 사람은 총 고르기
                gun = change_gun(nx, ny, gun)
                players[num] = [nx, ny, d, s, gun]
                # 진 사람 이동
                tx, ty, td, tg = lose_move(target_idx, tx, ty, td, tg)
                tg = change_gun(tx, ty, tg)
                # 위치 업데이트
                players[target_idx] = [tx, ty, td, ts, tg]

        # 플레이어가 없을 시
        else:
            # 총이 있다면 총 들기
            gun = change_gun(nx, ny, gun)
            players[num] = [nx, ny, d, s, gun]
        # print(players)
        # print(num)

for s in scores:
    print(s, end=' ')
```
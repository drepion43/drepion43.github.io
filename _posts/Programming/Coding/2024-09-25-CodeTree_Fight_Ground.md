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
인기 게임인 싸움땅은 다음과 같은 방식으로 진행됩니다. 게임은 n * n 크기의 격자에서 진행되며, 각각의 격자에는 무기들이 있을 수 있습니다. 초기에는 무기들이 없는 빈 격자에 플레이어들이 위치하며 각 플레이어는 초기 능력치를 가집니다. 각 플레이어의 초기 능력치는 모두 다릅니다. 게임은 다음과 같은 방식으로 진행됩니다.   
아래 그림에서 빨간색 배경의 숫자는 총의 경우 공격력을, 플레이어의 경우 초기 능력치를 의미하며, 노란색 배경의 숫자는 플레이어의 번호를 의미합니다.
<br>
하나의 라운드는 다음의 과정에 걸쳐 진행됩니다.   
1-1. 첫 번째 플레이어부터 순차적으로 본인이 향하고 있는 방향대로 한 칸만큼 이동합니다. 만약 해당 방향으로 나갈 때 격자를 벗어나는 경우에는 정반대 방향으로 방향을 바꾸어서 1만큼 이동합니다.
<br>
2-1. 만약 이동한 방향에 플레이어가 없다면 해당 칸에 총이 있는지 확인합니다. 총이 있는 경우, 해당 플레이어는 총을 획득합니다. 플레이어가 이미 총을 가지고 있는 경우에는 놓여있는 총들과 플레이어가 가지고 있는 총 가운데 공격력이 더 쎈 총을 획득하고, 나머지 총들은 해당 격자에 둡니다.   
2-2-1. 만약 이동한 방향에 플레이어가 있는 경우에는 두 플레이어가 싸우게 됩니다. 해당 플레이어의 초기 능력치와 가지고 있는 총의 공격력의 합을 비교하여 더 큰 플레이어가 이기게 됩니다. 만일 이 수치가 같은 경우에는 플레이어의 초기 능력치가 높은 플레이어가 승리하게 됩니다. 이긴 플레이어는 각 플레이어의 초기 능력치와 가지고 있는 총의 공격력의 합의 차이만큼을 포인트로 획득하게 됩니다.   
2-2-2. 진 플레이어는 본인이 가지고 있는 총을 해당 격자에 내려놓고, 해당 플레이어가 원래 가지고 있던 방향대로 한 칸 이동합니다. 만약 이동하려는 칸에 다른 플레이어가 있거나 격자 범위 밖인 경우에는 오른쪽으로 90도씩 회전하여 빈 칸이 보이는 순간 이동합니다. 만약 해당 칸에 총이 있다면, 해당 플레이어는 가장 공격력이 높은 총을 획득하고 나머지 총들은 해당 격자에 내려 놓습니다.    
2-2-3. 이긴 플레이어는 승리한 칸에 떨어져 있는 총들과 원래 들고 있던 총 중 가장 공격력이 높은 총을 획득하고, 나머지 총들은 해당 격자에 내려 놓습니다.    
<br>
위 과정을 1번부터 n번 플레이어까지 순차적으로 한 번씩 진행하면 1 라운드가 끝나게 되고, 그 결과는 다음과 같습니다.   
1번 라운드에 걸쳐 전체 플레이어가 획득한 포인트는 1번 사람부터 n번 사람까지 순서대로 [1, 0, 0, 0]입니다.   
위의 과정을 한번더 반복하여 나온 2번 라운드 결과는 다음과 같으며, 2번 라운드 이후 획득한 포인트 역시 1번 라운드와 동일하게 [1, 0, 0, 0]이 됩니다.
<br>
k 라운드 동안 게임을 진행하면서 각 플레이어들이 획득한 포인트를 출력하는 프로그램을 작성해보세요.


### 입력
첫 번째 줄에 n, m, k가 공백을 사이에 두고 주어집니다. n은 격자의 크기, m은 플레이어의 수, k는 라운드의 수를 의미합니다.   

이후 n개의 줄에 걸쳐 격자에 있는 총의 정보가 주어집니다. 각 줄에는 각각의 행에 해당하는 n개의 수가 공백을 사이에 두고 주어집니다. 숫자 0은 빈 칸, 0보다 큰 값은 총의 공격력을 의미합니다.   

이후 m개의 줄에 걸쳐 플레이어들의 정보 x, y, d, s가 공백을 사이에 두고 주어집니다. (x, y)는 플레이어의 위치, d는 방향, s는 플레이어의 초기 능력치를 의미하고 각 플레이어의 초기 능력치는 모두 다릅니다. 방향 d는 0부터 3까지 순서대로 ↑, →, ↓, ←을 의미합니다.   

각 플레이어의 위치는 겹쳐져 주어지지 않으며, 플레이어의 초기 위치에는 총이 존재하지 않습니다.   

- 2 ≤ n ≤ 20
- 1 ≤ m ≤ min(n^2, 30)
- 1 ≤ k ≤ 500
- 1 ≤ 총의 공격력 ≤ 100,000
- 1 ≤ s ≤ 100
- 1 ≤ x, y ≤ n

### 해결법
이번 문제는 시뮬레이션으로 정해진 규칙대로 동작하게 구현하면 됩니다.   
m명의 플레이어의 좌표, 기본 능력치, 바라보는 방향이 입력으로 주어집니다. 그 후 방향대로 1개의 라운드동안 m명의 플레이어가 움직이는데, 이 때 움직인 칸에 총이 있으면 가장 높은 공격력의 총을 줍습니다. 그 후 총의 공격력과 자신의 기본 능력치를 더한 최종 공격력을 얻게됩니다. 따라서 우선 모든 플레이어가 움직이는 move 함수를 구현했습니다. 이 때 총은 3차원 행렬로 구현하여, (x,y)에 여러개의 총이 존재할 수도 있어, (x,y)에 리스트로 총들을 가지고 있습니다. 이 총 리스트는 가장 공격력이 높은 순서대로 내림차순 정렬 진행해줬습니다. 플레이어가 이동한 칸에 총이 있는지를 확인 후 총이 있다면, 본인이 들고 있는 총의 공격력과 이동한 칸에 있는 가장 높은 공격력의 총과 비교하여 이동한 칸의 있는 총의 공격력이 더 높다면, 해당 총과 변경하는 작업을 수행하는 함수인 change_gun를 구현했습니다.    
그 후, 만약 플레이어가 이동하는 방향에 다른 플레이어가 존재시, 그 플레이어와 싸움을 시작하는데, 싸움은 최종 공격력이 더 높은 플레이어가 이기게 됩니다. 이긴 플레이어는 그 자리를 유지할 수 있고, 진 플레이어는 바라보는 방향으로 다시 이동해야하며, 그 칸에 들고있던 총을 내려놓고 이동하게됩니다. 따라서, 우선 이동할 좌표를 move함수로 얻어낸 후, 해당 좌표에 다른 플레이어가 있는지 확인하는 함수인 match_player를 구현했습니다. match_player는 만약 플레이어가 존재한다면 플레이어의 번호, 없다면 -1를 return하게 했습니다.    
만약, 다른 플레이어가 존재할시 싸움을 해야한다고 햇으니, 싸움을 하는 함수인 fight함수를 구현했습니다. fight함수는 플레이어의 총 공격력을 서로 비교하여, 더 높은 쪽이 이기게되는데 이 때, 진 플레이어의 번호, 공격력의 차를 return하게 됩니다. 그 이유는 진 플레이어는 다시 이동을 해야하기 때문입니다. 그 후 진 플레이어는 기존 움직인 방향으로 움직이긴하는데, 만약 벽이나 다시 이동하는 방향에 또 다른 플레이어가 존재할시, 시계방향 90도로 회전하여 이동하게 됩니다. 그 함수를 lose_move로 구현했습니다. lose_move에서는 우선 진 플레이어가 바라보는 방향으로 이동하는데, 그 이동한 칸이 벽 밖이거나, 다른 플레이어가 존재할 시, 이동하기전 칸에서 시계방향 90도로 회전하며 갈 수 있는 방향이 존재하면 바로 그 칸으로 이동하게 break문으로 구현했습니다.   
이제 구현한 함수들을 이용하여 최종적으로 시뮬레이션을 작동시키게 콜하면 됩니다.    
1. 각 플레이어가 모두 바라보는 방향으로 이동하는 move함수를 호출하여 이동할 칸의 좌표를 얻어냅니다.   
2. match_player 함수로 이동할 칸에 다른 플레이어가 존재하는지 확인합니다.   
    - 만약 다른 플레이어가 없다면, 이동한 칸에 있는 총과 본인이 들고 있는 총과 비교하여 총을 바꿔두는 함수인 change_gun을 호출 한 후, player의 값들을 변경시켜줍니다.   
    - 만약 다른 플레이어가 있다면, 우선 다른 플레이어와 싸움을 수행하는 fight함수를 콜해, 어떤 플레이어가 지는지와 점수가 몇점인지를 얻습니다.   
3. 다른 플레이어와 싸움을 한 후, 얻은 진 플레이어의 번호를 기반으로, 이긴 플레이어는 점수를 얻고, 진 플레이어는 들고있던 총을 다시 내려놓습니다. 이 때, 진 플레이어가 내려놓은 총과 이긴 플레이어가 들고있는 총과 비교하여 더 높은 공격력의 총을 이긴 플레이어는 변경합니다.   
4. 진 플레이어는 lose_move함수를 통해 다시 이동합니다. 그 후 각 플레이어의 좌표를 업데이트시켜줍니다.


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
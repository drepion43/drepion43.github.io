---
title:  "[코드트리]나무 박멸"
categories: Coding
tag: [Coding, Coding Test, Baekjoon, Python, Simulation]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
date: 2024-08-01
toc_sticky: true
---

## 나무 박멸

<https://www.codetree.ai/training-field/frequent-problems/problems/tree-kill-all/description?page=4&pageSize=5>

### 문제

n * n 격자에 나무의 그루 수와 벽의 정보가 주어집니다.
<br>
나무의 성장과 번식력이 좋아서 제초제를 뿌려 나무의 성장을 억제하고자 합니다. 제초제의 경우 k의 범위만큼 대각선으로 퍼지며, 벽이 있는 경우 가로막혀서 전파되지 않습니다. 다음과 같이 초기 조건이 주어진다고 가정할 때, 1년동안 나무의 성장과 억제는 다음과 같이 이뤄집니다.
<br>
<div style="text-align : center;">
<img src="../../../assets/images/Coding/2024-08-01-CodeTree_tree eradication/fig1.png" alt="codetree 출처" style="zoom:20%;" />    
</div>
1. 인접한 네 개의 칸 중 나무가 있는 칸의 수만큼 나무가 성장합니다. 성장은 모든 나무에게 동시에 일어납니다.

<div style="text-align : center;">
<img src="../../../assets/images/Coding/2024-08-01-CodeTree_tree eradication/fig2.png" alt="codetree 출처" style="zoom:20%;" />    
</div>
2. 기존에 있었던 나무들은 인접한 4개의 칸 중 벽, 다른 나무, 제초제 모두 없는 칸에 번식을 진행합니다. 이때 각 칸의 나무 그루 수에서 총 번식이 가능한 칸의 개수만큼 나누어진 그루 수만큼 번식이 되며, 나눌 때 생기는 나머지는 버립니다. 번식의 과정은 모든 나무에서 동시에 일어나게 됩니다.

<div style="text-align : center;">
<img src="../../../assets/images/Coding/2024-08-01-CodeTree_tree eradication/fig3.png" alt="codetree 출처" style="zoom:20%;" />    
</div>
3. 각 칸 중 제초제를 뿌렸을 때 나무가 가장 많이 박멸되는 칸에 제초제를 뿌립니다. 나무가 없는 칸에 제초제를 뿌리면 박멸되는 나무가 전혀 없는 상태로 끝이 나지만, 나무가 있는 칸에 제초제를 뿌리게 되면 4개의 대각선 방향으로 k칸만큼 전파되게 됩니다. 단 전파되는 도중 벽이 있거나 나무가 아얘 없는 칸이 있는 경우, 그 칸 까지는 제초제가 뿌려지며 그 이후의 칸으로는 제초제가 전파되지 않습니다. 제초제가 뿌려진 칸에는 c년만큼 제초제가 남아있다가 c+1년째가 될 때 사라지게 됩니다. 제초제가 뿌려진 곳에 다시 제초제가 뿌려지는 경우에는 새로 뿌려진 해로부터 다시 c년동안 제초제가 유지됩니다.

<div style="text-align : center;">
<img src="../../../assets/images/Coding/2024-08-01-CodeTree_tree eradication/fig4.png" alt="codetree 출처" style="zoom:20%;" />    
</div>
k가 2일 때, 각각의 칸에 제초제가 뿌려진 경우 박멸되는 나무의 총 그루 수는 다음과 같습니다.
<br>
3행 4열이 가장 많은 나무를 박멸시키는 것을 알 수 있으며, 해당 칸에 제초제를 뿌리게 됩니다. 만약 박멸시키는 나무의 수가 동일한 칸이 있는 경우에는 행이 작은 순서대로, 만약 행이 같은 경우에는 열이 작은 칸에 제초제를 뿌리게 됩니다.
<br>
3행 4열에 제초제를 뿌린 이후에는 다음과 같이 변하게 됩니다.   
<div style="text-align : center;">
<img src="../../../assets/images/Coding/2024-08-01-CodeTree_tree eradication/fig5.png" alt="codetree 출처" style="zoom:20%;" />    
</div>
각 3개의 과정이 1년에 걸쳐 진행된다고 했을 때, m년 동안 총 박멸한 나무의 그루 수를 구하는 프로그램을 작성해보세요.   
위의 경우에서 제초제가 1년간 유지된다고 가정했을 때, 그 다음 1년동안의 과정을 그려보면 다음과 같습니다.

### 입력
첫 번째 줄에 격자의 크기 n, 박멸이 진행되는 년 수 m, 제초제의 확산 범위 k, 제초제가 남아있는 년 수 c가 공백을 사이에 두고 주어집니다.   
이후 n개의 줄에 걸쳐 각 칸의 나무의 그루 수, 벽의 정보가 주어집니다. 총 나무의 그루 수는 1 이상 100 이하의 수로, 빈 칸은 0, 벽은 -1으로 주어지게 됩니다.   

- 5 ≤ n ≤ 20
- 1 ≤ m ≤ 1000
- 1 ≤ k ≤ 20
- 1 ≤ c ≤ 10

### 해결법
우선 문제에 주어진대로 크게 4가지로 구분했습니다. 나무의 성장, 나무의 번식, 제초제 위치선정 및 뿌리기, 제초제 유지기간 감소   
나무의 성장과 번식은 상,하,좌,우 4방향으로 움직이고, 제초재는 상좌, 상우, 하좌, 하우 대각선 4방향으로 확산됩니다. 이를 위해 (dx, dy), (ddx, ddy) 리스트를 따로 정의했습니다.    
처음 주어지는 보드판에서 -1은 벽을 의미하므로, -1이 입력으로 들어오는, 즉, 벽의 위치는 wall_pos 리스트에 따로 보관을 해두었습니다. 왜냐하면, **제초제 뿌린 위치를 기존 보드판에서 음수**로 표현할려고 했기 때문입니다. 0은 빈칸, 0보다 큰 곳은 나무가 존재하는 곳을 의미합니다.   
1번인 나무의 성장은 인접한 상,하,좌,우 4방향에 존재하는 나무의 개수만큼 성장합니다. 최대 4까지 성장할 수 있습니다.    
2번인 나무의 번식은 인접한 칸이 빈칸인 경우 빈칸의 개수만큼 균등하게 번식합니다. 이 때, 번식은 모두 동시에 일어나기 때문에, 번식하는 개수를 담을 tmp_board라는 리스트를 함수내의 지역변수로 선언하며 번식할 곳을 브르투포스로 다 돈 후 tmp_board와 원본 board간의 add를 해주어 번식 부분을 해결했습니다. 이 때 번식은 겹치는 경우 누적이 가능합니다.   
3번 제초제 위치 선정 부분에서 처음에는 문제를 잘못읽어 제초제가 확산이 가능한 지점이면 계속해서 확산하는 줄알고 DFS로 구현을 했습니다. 하지만, 문제를 다시 읽어보니 **확산 범위까지만 확산**을 하는데, 이 때, **벽이나 빈칸을 만나면 그 지점까지만 확산을 한다는 것**으로 재이해했습니다. 우선 여기서, 당연히 가장 많이 박멸시킬 수 있는 위치를 찾아야하며 이 때 **박멸시킬 수 있는 개수가 동일할 경우, 행 -> 열이 작은 순**인 지점을 선정합니다. 따라서 리스트에 박멸 개수, 박멸 위치를 저장하여 sort하여 가장 앞 부분인 0번 index를 추출하였습니다. 추가적으로 확산하는 범위는 **확산 범위의 반복문 후, 방향의 반목문**을 이용했습니다. 약간 DFS같은 느낌으로 생각하시면 될 것 같습니다.    
그럼 이제 그 지점을 기반으로 제초제를 확산시키면 됩니다. 이 때 항상 생각해야할 것이 나무가 존재하는 지점에 제초제를 뿌려야하니 **그 지점의 값이 0보다 큰지 확인**했습니다. 그리고 제초제를 뿌리는데 이 때 **제초제를 뿌리는 위치에 값을 -c로 변경을 해주는데요. 이 때 현재 선정한 위치는 제초제 유지기간 감소에 포함시키 않기 위해 따로 kill_lst라는 리스트를 두어 여기에 저장**시켜놓았습니다.   
이제 제초제 유지기간을 감소시키는 함수에서 **벽인 곳은 제외, 현재 뿌려진 kill_lst 위치는 제외**를 하고 나머지 음수 값에 대해 +1을 해주어 유지기간을 감소 시켰습니다.   
마지막으로 시뮬레이션 동작 순서는 문제 그대로 1. 나무 성장 2. 나무 번식 3. 제초제 뿌리기 4. 제초제 유지기간 감소(처음 해는 pass)
```python
from collections import deque

# 나무의 성장 : 인접한 나무의 수만큼 1년동안 개수 증가(최대 4 증가)
# 나무의 번식 : 비어 잇는 인접한 4칸으로 번식(n등분하여 번식(나머지는 버림), 겹치는 곳은 더해줌)

# 제초제 : 나무가 가장 많이 박멸되는 곳에 뿌림(4개의 대각선으로 k만큼 전파), c년만큼 제초제 유지, c+1년에 제초제 사라짐
# 제초제 : 박멸시키는 개수가 동일한 지점이 존재할 시, 행 -> 열이 작은 순으로

# 격자 크기, 박멸 진행년수, 제초제 확산범위, 제초제 유지 기간
n, m, k, c = map(int, input().split())

# 벽위치
wall_pos = []

# 원본 판
board = []
for x in range(n):
    data = list(map(int, input().split()))
    for y in range(n):
        if data[y] == -1:
            wall_pos.append((x, y))
    board.append(data)

# print(board)

# 나무 인접 방향
dx = [1, -1, 0, 0]
dy = [0, 0, 1, -1]

# 제초제 인접
ddx = [1, 1, -1, -1]
ddy = [1, -1, 1, -1]

# 나무 성장
def grow_tree(_board):
    for x in range(n):
        for y in range(n):
            # 나무가 있는 위치
            if _board[x][y] > 0:
                cnt = 0
                # 인접한 위치 나무 개수 확인
                for i in range(4):
                    nx, ny = x + dx[i], y + dy[i]
                    if 0 <= nx < n and 0 <= ny < n:
                        if _board[nx][ny] > 0:
                            cnt += 1
                _board[x][y] += cnt
    return _board

# 나무 번식
def spread_tree(_board):
    # 번식할 나무
    tmp_board = [[0] * n for _ in range(n)]
    for x in range(n):
        for y in range(n):
            # 나무가 있는 위치
            if _board[x][y] > 0:
                cnt = 0
                # 인접한 위치에 빈칸 개수 확인
                for i in range(4):
                    nx, ny = x + dx[i], y + dy[i]
                    if 0 <= nx < n and 0 <= ny < n:
                        if _board[nx][ny] == 0:
                            cnt += 1
                # 인접한 위치 빈칸에 나무 번식
                for i in range(4):
                    nx, ny = x + dx[i], y + dy[i]
                    if 0 <= nx < n and 0 <= ny < n:
                        if _board[nx][ny] == 0:
                            # 번식할 지점에 나무들
                            tmp_board[nx][ny] += _board[x][y] // cnt
    # 번식 한 곳 더해주기
    _board = [[a + b for a, b in zip(row1, row2)] for row1, row2 in zip(_board, tmp_board)]
    return _board

def loop_position(_board, _x, _y, _cnt):
    # 나무가 없거나 벽이면 전파 종료
    if _board[_x][_y] == 0 or _board[_x][_y] == -1:
        return



# 제초제 뿌릴 위치 선장
def set_position(_board):
    global answer
    max_val, max_x, max_y = 0, -1, -1
    max_lst = []
    for x in range(n):
        for y in range(n):
            # 나무 있는 위치
            if _board[x][y] > 0:
                # 현재 지점 나무 개수
                cur_cnt = _board[x][y]
                # 대각선 방향으로 확산
                for i in range(4):
                    # 제초제 범위만큼 확산
                    for j in range(1, k + 1):
                        nx, ny = x + (ddx[i] * j), y + (ddy[i] * j)
                        # 보드 크기 내부 + 나무가 있는 곳으로만 제초제 확산 가능
                        if 0 <= nx < n and 0 <= ny < n:
                            # 나무가 있는 경우
                            if _board[nx][ny] > 0:
                                cur_cnt += _board[nx][ny]
                            # 확산 범위까지만
                            else:
                                break
                # 제초제뿌릴시 가장 많이 제거되는 위치 찾기
                if max_val <= cur_cnt:
                    max_val, max_x, max_y = cur_cnt, x, y
                    max_lst.append([max_val, max_x, max_y])
    # 가장 큰 순서로 정렬
    max_lst = sorted(max_lst, key=lambda x:(-x[0], x[1], x[2]))
    if len(max_lst) > 0:
        max_val, max_x, max_y = max_lst[0]
    # 제거되는 나무 수
    answer += max_val

    # print('제초제 뿌릴 위치', max_x, max_y)
    # 제초제 뿌리는 위치
    kill_lst = []
    # 가장 크게 제거되는 지점에 제초제 뿌리기
    # 제초제 뿌릴 위치는 당연히 나무가 존재해야함
    if _board[max_x][max_y] > 0:
        # 제초제 유해기간은 음수로 지정
        _board[max_x][max_y] = -c
        kill_lst.append((max_x, max_y))
        # 전파된 제초제 위치
        for i in range(4):
            for j in range(1, k + 1):
                nnx, nny = max_x + (ddx[i] * j), max_y + (ddy[i] * j)
                if 0 <= nnx < n and 0 <= nny < n:
                    # 벽 위치의 경우에는 종료
                    if (nnx, nny) in wall_pos:
                        break
                    # 나무가 있는 곳
                    elif _board[nnx][nny] > 0:
                        _board[nnx][nny] = -c
                        kill_lst.append((nnx, nny))
                    # 빈 칸이나 이미 제초제가 뿌려진 곳에는 다시 제초제 뿌림
                    else:
                        _board[nnx][nny] = -c
                        kill_lst.append((nnx, nny))
                        break
    return _board, kill_lst

# 제초제는 매년 1년마다 유지기간이 감소함
def decrease_maintain(_board, _kill_lst):
    for x in range(n):
        for y in range(n):
            # 벽인 경우 무시
            if (x, y) in wall_pos:
                continue
            elif (x, y) in _kill_lst:
                continue
            elif _board[x][y] < 0:
                _board[x][y] += 1
    return _board

# 죽인 나무 수
answer = 0

# m년동안 진행
for t in range(m):
    # 1. 나무 성장
    board = grow_tree(board)
    # print('나무 성장 : ')
    # for i in range(n):
    #     print(board[i])
    # 2. 나무 번식
    board = spread_tree(board)
    # print('나무 번식 : ')
    # for i in range(n):
    #     print(board[i])
    # 3. 제초제 뿌림
    board, kill_lst = set_position(board)
    # print('나무 제거 : ')
    # for i in range(n):
    #     print(board[i])
    # 4. 제초제 유지기간 감소
    if t > 0:
        board = decrease_maintain(board, kill_lst)
    # print()
print(answer)

```
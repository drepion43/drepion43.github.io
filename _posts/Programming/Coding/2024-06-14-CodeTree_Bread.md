---
title:  "[코드트리]빵"
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

## 빵

<https://www.codetree.ai/training-field/frequent-problems/problems/codetree-mon-bread/description?page=1&pageSize=20>

### 문제
최근 코드트리 빵이 전국적으로 인기를 얻어 편의점에서 해당 빵을 구하기 힘들어졌습니다. 빵을 구하고자 하는 m명의 사람이 있는데, 1번 사람은 정확히 1분에, 2번 사람은 정확히 2분에, ..., m번 사람은 정확히 m 분에 각자의 베이스캠프에서 출발하여 편의점으로 이동하기 시작합니다. 사람들은 출발 시간이 되기 전까지 격자 밖에 나와있으며, 사람들이 목표로 하는 편의점은 모두 다릅니다. 이 모든 일은 n*n 크기의 격자 위에서 진행됩니다.   

코드트리 빵을 구하고 싶은 사람들은 다음과 같은 방법으로 움직입니다. 이 3가지 행동은 총 1분 동안 진행되며, 정확히 1, 2, 3 순서로 진행되어야 함에 유의합니다.   

- 격자에 있는 사람들 모두가 본인이 가고 싶은 편의점 방향을 향해서 1 칸 움직입니다. 최단거리로 움직이며 최단 거리로 움직이는 방법이 여러가지라면 ↑, ←, →, ↓ 의 우선 순위로 움직이게 됩니다. 여기서 최단거리라 함은 상하좌우 인접한 칸 중 이동가능한 칸으로만 이동하여 도달하기까지 거쳐야 하는 칸의 수가 최소가 되는 거리를 뜻합니다.

- 만약 편의점에 도착한다면 해당 편의점에서 멈추게 되고, 이때부터 다른 사람들은 해당 편의점이 있는 칸을 지나갈 수 없게 됩니다. 격자에 있는 사람들이 모두 이동한 뒤에 해당 칸을 지나갈 수 없어짐에 유의합니다.

- 현재 시간이 t분이고 t ≤ m를 만족한다면, t번 사람은 자신이 가고 싶은 편의점과 가장 가까이 있는 베이스 캠프에 들어갑니다. 여기서 가장 가까이에 있다는 뜻 역시 1에서와 같이 최단거리에 해당하는 곳을 의미합니다. 가장 가까운 베이스캠프가 여러 가지인 경우에는 그 중 행이 작은 베이스캠프, 행이 같다면 열이 작은 베이스 캠프로 들어갑니다. t번 사람이 베이스 캠프로 이동하는 데에는 시간이 전혀 소요되지 않습니다.

이때부터 다른 사람들은 해당 베이스 캠프가 있는 칸을 지나갈 수 없게 됩니다. t번 사람이 편의점을 향해 움직이기 시작했더라도 해당 베이스 캠프는 앞으로 절대 지나갈 수 없음에 유의합니다. 마찬가지로 격자에 있는 사람들이 모두 이동한 뒤에 해당 칸을 지나갈 수 없어짐에 유의합니다.   
   

아래와 같이 n = 5, m = 3, 베이스 캠프 4곳, 편의점 3곳이 주어진 경우에 대해서 생각해보겠습니다.
### 입력
첫 번째 줄에는 격자의 크기 n과 사람의 수 m이 공백을 사이에 두고 주어집니다.   
이후 n개의 줄에 걸쳐 격자의 정보가 주어집니다. 각 줄에 각각의 행에 해당하는 n개의 수가 공백을 사이에 두고 주어집니다.   
0의 경우에는 빈 공간, 1의 경우에는 베이스캠프를 의미합니다.   

이후 m개의 줄에 걸쳐 각 사람들이 가고자 하는 편의점 위치의 행 x, 열 y의 정보가 공백을 사이에 두고 주어집니다.   
각 사람마다 가고 싶은 편의점의 위치는 겹치지 않으며, 편의점의 위치와 베이스캠프의 위치도 겹치지 않습니다.   

- $2 \le n \le 15$
- $1 \le m \le min(n^2, 30)$
- $m \le Base \; Camp \; Num \le n^2 - m$

### 해결법
우선 시작시 모든 사람들은 격자 밖에 있다고 하니, 모든 사람들의 좌표를 세팅이 안된 (-1, -1)로 세팅을 했습니다. 그 후 1분일 때, 1번째 사람이 1번째 편의점에서 가장 가까운 베이스캠프를 선택해야하는데, 이 때 편의점에서 가장 가까운 베이스캠프를 찾는 함수인 BFS함수를 1개 설정했습니다. 이 때, 각 보드판에서 **이미 선정된 베이스캠프**와 **사람이 편의점에 도착했을 때 해당 편의점**은 지나갈 수가 없기 때문에, 이미 선정된 베이스캠프는 해당 번호의 사람의 -를 붙이고, 편의점에 도착한것은 해당 보드판에서 좌표를 2로 설정하여 그 경우에는 지나갈 수 없는 조건을 걸었습니다. 그럼 이제, 각 사람들이 목표하는 편의점으로 가기 위한 최단 거리인 베이스캠프를 선정하는 부분은 끝이 났습니다.   
이제, 사람들의 이동을 고려해야합니다. 1분마다 순서에 맞게 사람이 베이스캠프로 들어오는데, 이미 들어온 사람은 그 시간에 해당 편의점을 향해 한칸 전진합니다. 따라서 사람이 이동하는 BFS(move) 함수를 추가적으로 작성했습니다. 이 때, **최단거리로 가는 경로**를 구해 현재 위치에서 어디로 가야 최단 거리로 갈 수 있는지를 return합니다. 즉, 다음 이동해야할 좌표입니다. 이 좌표로 이동하는 사람의 좌표를 업데이트했고, 그 사람이 편의점에 도착할 시 보드판의 좌표는 2로 업데이트했습니다. 추가적으로 **현재 시간대에서는 편의점에 도착해도 갈 수 있다**는 점을 고려하여, 보드판 업데이틑 모두 이동한 후 업데이트를 진행했습니다.   
```python
from collections import deque

n, m = map(int, input().split())

board = []
base_camps = []

for x in range(n):
    data = list(map(int, input().split()))
    board.append(data)
    # 베이스 캠프 위치 얻기
    for y, _ in enumerate(data):
        if data[y] == 1:
            base_camps.append([x, y])

# 가고싶은 편의점
markets = []
for _ in range(m):
    x, y = map(int, input().split())
    markets.append([x-1, y-1])

people_arrive = [False] * m

# 상 좌 우 하
dx = [-1, 0, 0, 1]
dy = [0, -1, 1, 0]

# 편의점에서 베이스 캠프와 가장 가까운 거리 구하기(bfs) -> input : 몇순위 편의점, 편의점의 좌표(1순위, 2순위 대로 들어옴)
def bfs(_idx, _x, _y, _board):
    # 거리와 좌표 담을 리스트
    distances = []
    q = deque()
    q.append([_x, _y])
    visited = [[-1] * n for _ in range(n)]
    visited[_x][_y] = 0
    while q:
        x, y = q.popleft()
        if _board[x][y] == 1:
            # 후보가 될 수 있는 베이스캠프일 경우 거리, 좌표
            distances.append([visited[x][y], x, y])

        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            # 만약 board가 음수라면 해당 편의점과 가장 가까운 베이스캠프를 설정되었다는 뜻 -> 1순위 편의점의 베이스캠프 : -1 (or 편의점에 도착한 사람이 있는 경우: 2), 2순위 편의점의 베이스캠프 : -2
            if 0 <= nx < n and 0 <= ny < n and _board[nx][ny] >= 0 and _board[nx][ny] != 2 and visited[nx][ny] == -1:
                q.append([nx, ny])
                visited[nx][ny] = visited[x][y] + 1
    # 베이스캠프 거리순으로 정렬(거리, 행, 열 작은 순으로)
    distances.sort(key=lambda x:(x[0], x[1], x[2]))
    min_dis, min_x, min_y = distances[0]
    # 편의점에서 최단 거리인 베이스 캠프의 좌표(어떤 편의점과 가까운지)
    return _board, min_x, min_y, _idx

# 사람 이동
def move(_x, _y, _board, tx, ty):
    q = deque()
    q.append([_x, _y])
    visited = [[-1] * n for _ in range(n)]
    visited[_x][_y] = 0
    # 이전에 위치 저장
    parents = [[None] * n for _ in range(n)]

    while q:
        x, y = q.popleft()
        if x == tx and y == ty:
            # 도착지까지의 경로를 닮은 리스트
            path = []
            current = [x, y]
            # 현재 위치에서 이전 위치값을 대입하며 None이 될때까지 반복
            while current is not None:
                path.append(current)
                current = parents[current[0]][current[1]]
            path.reverse()
            # 다음 이동해야할 위치
            next_x, next_y = path[1]
            return next_x, next_y

        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            if 0 <= nx < n and 0 <= ny < n and _board[nx][ny] >= 0 and _board[nx][ny] != 2 and visited[nx][ny] == -1:
                q.append([nx, ny])
                visited[nx][ny] = visited[x][y] + 1
                # 이전 위치 저장
                parents[nx][ny] = [x, y]

    # 다음 이동할 수 있는 곳이 없을 때
    return -1, -1

# 모든 사람이 편의점 도착했는지 확인
def check_finish():
    for i in range(m):
        if people[i] != markets[i]:
            return False
    return True


t = 0
# 사람 좌표(m명 사람 좌표 초기 세팅)
people = [[-1, -1] for _ in range(m)]

# 편의점 번호, 베이스캠프 좌표, 편의점 좌표
camp_markets = []
while True:
    # 베이스캠프를 선택한 사람들은 이동
    for i in range(m):
        # 아직 선택을 안한 사람들은 패스
        if people[i][0] == -1 and people[i][1] == -1:
            continue
        # 해당 사람이 가야하는 편의점(목적지) 좌표
        target_x, target_y = markets[i]
        # 현재 사람 위치가 편의점인지 확인
        if people[i][0] == target_x and people[i][1] == target_y:
            continue
        # 이동
        next_x, next_y = move(people[i][0], people[i][1], board, target_x, target_y)
        people[i] = [next_x, next_y]

    for i in range(m):
        if people[i] == markets[i]:
            # 편의점에 도착함
            # 모든 사람들이 이동한 뒤에 이동할 수 없게됨
            board[people[i][0]][people[i][1]] = 2

    # m번 사람까지의 시간이 안지났을 때, 베이스캠프를 선택함
    if t < m:
        # 편의점 좌표
        mx, my = markets[t]
        # 베이스캠프 좌표
        board, min_x, min_y, idx = bfs(t + 1, mx, my, board)
        camp_markets.append([idx, min_x, min_y, mx, my])
        # 처음 사람은 베이스캠프로 순간 이동함
        people[idx - 1] = [min_x, min_y]
        # 모든 사람들이 이동한 뒤에 이동할 수 없게됨
        board[min_x][min_y] = -idx


    t += 1
    if check_finish():
        break
    
print(t)



```

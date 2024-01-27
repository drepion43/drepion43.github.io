---
title:  "[프로그래머스]258711"
categories: Coding
tag: [Coding, Coding Test, Baekjoon, Python, Simulation, BFS, DFS, Combination]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
comments: true
date: 2024-01-26
toc_sticky: true
---

## 도넛과 막대 그래프(2024 KAKAO WINTER INTERSHIP)

[도넛과 막대 그래프](https://school.programmers.co.kr/learn/courses/30/lessons/258711)

### 문제
도넛 모양 그래프, 막대 모양 그래프, 8자 모양 그래프들이 있습니다. 이 그래프들은 1개 이상의 정점과, 정점들을 연결하는 단방향 간선으로 이루어져 있습니다.   
- 크기가 n인 도넛 모양 그래프는 n개의 정점과 n개의 간선이 있습니다. 도넛 모양 그래프의 아무 한 정점에서 출발해 이용한 적 없는 간선을 계속 따라가면 나머지 n-1개의 정점들을 한 번씩 방문한 뒤 원래 출발했던 정점으로 돌아오게 됩니다. 도넛 모양 그래프의 형태는 다음과 같습니다.
제목 없는 다이어그램.drawio.png

- 크기가 n인 막대 모양 그래프는 n개의 정점과 n-1개의 간선이 있습니다. 막대 모양 그래프는 임의의 한 정점에서 출발해 간선을 계속 따라가면 나머지 n-1개의 정점을 한 번씩 방문하게 되는 정점이 단 하나 존재합니다. 막대 모양 그래프의 형태는 다음과 같습니다.
도넛과막대2.png

- 크기가 n인 8자 모양 그래프는 2n+1개의 정점과 2n+2개의 간선이 있습니다. 8자 모양 그래프는 크기가 동일한 2개의 도넛 모양 그래프에서 정점을 하나씩 골라 결합시킨 형태의 그래프입니다. 8자 모양 그래프의 형태는 다음과 같습니다.
8자그래프.drawio.png

도넛 모양 그래프, 막대 모양 그래프, 8자 모양 그래프가 여러 개 있습니다. 이 그래프들과 무관한 정점을 하나 생성한 뒤, 각 도넛 모양 그래프, 막대 모양 그래프, 8자 모양 그래프의 임의의 정점 하나로 향하는 간선들을 연결했습니다.
그 후 각 정점에 서로 다른 번호를 매겼습니다.
이때 당신은 그래프의 간선 정보가 주어지면 생성한 정점의 번호와 정점을 생성하기 전 도넛 모양 그래프의 수, 막대 모양 그래프의 수, 8자 모양 그래프의 수를 구해야 합니다.

그래프의 간선 정보를 담은 2차원 정수 배열 edges가 매개변수로 주어집니다. 이때, 생성한 정점의 번호, 도넛 모양 그래프의 수, 막대 모양 그래프의 수, 8자 모양 그래프의 수를 순서대로 1차원 정수 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

### 제한 사항
- 1 ≤ edges의 길이 ≤ 1,000,000
    - edges의 원소는 [a,b] 형태이며, a번 정점에서 b번 정점으로 향하는 간선이 있다는 것을 나타냅니다.
    - 1 ≤ a, b ≤ 1,000,000
- 문제의 조건에 맞는 그래프가 주어집니다.
- 도넛 모양 그래프, 막대 모양 그래프, 8자 모양 그래프의 수의 합은 2이상입니다.

### 해결법



```python
def row_sum(arr, idx):
    result = 0
    for i in range(len(arr)):
        result += arr[i][idx]
    return result
def column_sum(arr, idx):
    result = 0
    for i in range(len(arr)):
        result += arr[idx][i]
    return result
def finish_compare(arr, i, j):
    arr[i][j] = -1
    arr[j][i] = -1

def solution(friends, gifts):
    answer = 0
    friend_count = len(friends)
    friend_num = {friend:i for i,friend in enumerate(friends)}
    friends_matrix = [[0]*friend_count for _ in range(friend_count) ]
    asnwers = [0 for _ in range(friend_count)]
    gift_scores = [0 for _ in range(friend_count)]
    
    for gift in gifts:
        gift_tmp = gift.split(' ')
        sender, receiver = gift_tmp[0], gift_tmp[1]
        sender_num, receiver_num = friend_num[sender], friend_num[receiver]
        friends_matrix[sender_num][receiver_num] += 1
    # 선물 지수 세팅
    for i in range(friend_count):
        gift_scores[i] = column_sum(friends_matrix, i) - row_sum(friends_matrix, i)
    print(gift_scores)
    # 선물 개수 세팅
    for i in range(friend_count):
        for j in range(friend_count):
            # 선물을 주고 받은 적이 없는 경우 선물 지수 확인을 통해 선물을 줄지 말지 결정
            if friends_matrix[i][j] == 0 and friends_matrix[j][i] == 0:
                if gift_scores[i] > gift_scores[j]:
                    asnwers[i] += 1
                    # 선물을 주거나 받았으면 해당 인원들끼리는 더 이상 상호작용 필요가 없으니 해당 인원들에 대해서는 볼 필요가 없으니 default인 -1로 세팅
                    finish_compare(friends_matrix, i, j)
                elif gift_scores[i] < gift_scores[j]:
                    asnwers[j] += 1
                    finish_compare(friends_matrix, i, j)
            # 선물을 주거나 받은적이 있는 인원일 때
            elif friends_matrix[i][j] > 0:
                # 선물을 주고 받은 개수가 같을 때, 선물 지수로 확인
                if friends_matrix[j][i] == friends_matrix[i][j]:
                    if gift_scores[i] > gift_scores[j]:
                        asnwers[i] += 1
                        finish_compare(friends_matrix, i, j)
                    elif gift_scores[i] < gift_scores[j]:
                        asnwers[j] += 1
                        finish_compare(friends_matrix, i, j)
                # 선물을 준 게 더 많은 경우, 해당 인원은 그 인원에게 선물을 받아야 함
                elif friends_matrix[j][i] < friends_matrix[i][j]:
                    asnwers[i] += 1
                    finish_compare(friends_matrix, i, j)
                else:
                    asnwers[j] += 1
                    finish_compare(friends_matrix, i, j)

    answer = max(asnwers)
    return answer
```
---
title:  "Extra Algorithm"
categories: Coding
tag: [Coding, Algorithm, Theory, Python]
tags: [Jekyll, MathJax]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
---

## 소수 Algorithm

```python
# 소수인지 확인
def is_prime(num):
    for i in range(2, num):
        if num % i == 0:
            return False
    return True
print(is_prime(10))
print(is_prime(11))
# False
# True
```

위의 경우 time complexity를 보는 경우 비효율적임 &rarr; 소수의 경우 $\sqrt{num} \; +\; 1$ 까지만 확인했을 때와 그 이후로 확인 했을 때와 같은 결과가 도출   
```python
from math import sqrt
def is_prime_imporve(x):
    for i in range(2,int(sqrt(x))+1):
        if x%i==0:
            return False
    return True

print(is_prime_imporve(10))
print(is_prime_imporve(11))
# False
# True
```

## 에라토스테네스의 체

 여러개의 수들 중 소수인지 아닌지를 판별   
① 2부터 n까지의 모든 자연수를 나열  
② 남은 수 중에서 아직 처리하지 않은 가장 작은 수 i를 찾는다  
③ 남은 수 중에서 i의 배수를 제거한다.(i 제외)   
④ ②, ③을 반복할 수 없을 때까지 반복한다   

```python
from math import sqrt

n = 100
array = [True] * (n+1)

for i in range(2, int(sqrt(n)) + 1):
    # i가 소수인 경우
    if array[i]:
        # i를 제외한 i의 배수 제거
        j = 2
        while i * j <= n:
            array[i*j] = False
            j+=1
for i in range(2,n+1):
    if array[i]:
        print(i, end=' ')
# 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97 
```

## 투포인터

리스트에 순차적으로 접근해야 할 때 2개의 점의 위치를 기록하면서 처리하는 알고리즘(특정한 합을 가지는 부분 연속 수열 찾기)     
① 시작점과 끝점이 첫 번째 원소의 인덱스를 가리키도록 한다  
② 현재 부분합이 M과 같다면 카운트한다  
③ 현재 부분합이 M보다 작으면 end를 1 증가시킨다.   
④ 현재 부분합이 M보다 크거나 같으면 start를 1 증가시킨다.   
⑤ 모든 경우를 확인할 때까지 ②~④번을 반복한다   

```python
n,m=5,5
data=[1,2,3,2,5]

count=0
sum_val=0
end=0

for start in range(n):
    #end 이동시키기
    while sum_val<m and end<n:
        sum_val+=data[end]
        end+=1
    if sum_val==m:
        count+=1
    sum_val-=data[start]
print(count)
# 3
```

정렬되어 있는 두 리스트의 합집합   
① 정렬된 리스트 A,B가 주어진다  
② 리스트 A에서 처리되지 않은 원소 중 가장 작은 원소를 i가 가르키도록 한다.  
③ 리스트 B에서 처리되지 않은 원소 중 가장 작은 원소를 j가 가르키도록 한다.   
④ A[i],B[j]중에서 더 작은 원소를 결과 리스트에 넣는다.  
⑤ A와B에서 더이상 할 수 없을 때까지 ②~④번을 반복한다    

```python
n, m = 3, 4
a = [1, 3, 5]
b = [2, 4, 6, 8]
result = [0] * (n + m)
i, j, k = 0, 0, 0

while i < n or j < m:
    # a의 원소가 b보다 작거나 j의 원소를 다 처리한 경우
    if j >= m or (i < n and a[i] < b[j]):
        result[k] = a[i]
        i += 1
    else:  # b의 원소가 a보다 작거나 i의 원소를 다 처리한 경우
        result[k] = b[j]
        j += 1
    k += 1

for i in result:
    print(i, end=' ')
# 1 2 3 4 5 6 8 
```

## 이진 탐색(bisect)

 정렬된 배열에서 특정원소 찾기   
특정 원소의 개수 찾는데에 유용   

```python
#bisect_left(a,x) : 리스트 a에서 x의 데이터를 삽입할 가장 왼쪽 인덱스를 찾는 메서드
from bisect import bisect_left,bisect_right

a=[1,2,4,4,8]
x=4

print(bisect_left(a,x))
print(bisect_right(a,x))
# 왼쪽에서부터 확인할 때 4가 나올 위치는 1 2 다음이니 2번째 index
# 우측에서부터 확인할 때 4가 나올 위치는 8 바로 왼쪽이니 4번째 index
# 2
# 4
```

```python
from bisect import bisect_left,bisect_right

# list에서 left_val과 right_val사이의 값을 가지는 value의 개수를 찾는 함수
def count_by_value(lst,left_val,right_val):
    left_i=bisect_left(lst,left_val)
    right_i=bisect_right(lst,right_val)
    print(left_i,right_i)
    return right_i-left_i

a=[1,2,3,3,3,3,4,4,8,9]
print(count_by_value(a,4,4))

print(count_by_value(a,-1,3))
```


# Algorithm
## 220822 220823 stack, 백트래킹, 부분집합, 순열, 분할 정복
### 목표
* 알고리즘 문제 풀이 방법 익히기
* back tracking 문제 풀이 방법 익히기
* 부분 집합 문제 풀이 방법 익히기
* 순열 문제 풀이 방법 익히기


### stack 활용: 계산기
* infix notation 중위표기법
  * x + y
* postfix notation 후위표기법
  * xy+
  * 연산 우선순위대로 진행
  * 연산자를 연산식 오른쪽 문자의 후위로 이동
* stack을 사용하여 
  * 중위표기법 $\leftrightarrow$ 후위표기법
  * 양방향 전환 해보기
* [예제: 후위 표기식](https://www.acmicpc.net/problem/1918)
* [예제: 후위 표기식2](https://www.acmicpc.net/problem/1935)


### back tracking 백트래킹
* 해를 찾는 도중에 해가 아닌 것 같으면(더 이상 진행 불가능하면), 되돌아가서 다시 해를 찾는 방법
  1. dfs 깊이 우선 탐색 진행
  2. 어떤 노드의 유망성을 검사하여(promising), 
  3. 유망하지 않다면 그 노드의 부모로 돌아가서(back tracking), 
  4. 다음 자식 노드 탐색 반복

* optimization 최적화 문제와 decision 결정 문제를 해결할 때 사용
* decision 결정 문제
  * 문제의 조건을 만족하는 해가 존재하는지 여부를 판단하는 문제
  * 미로찾기
  * n-Queen
  * Map coloring
  * subset sum
* promising 유망
  * 해당 경로가 해를 찾을 수 있는지 아닌지(유망한지 아닌지) 판단
* prunning 가지치기
  * 유망하지 않은 노드는 제외
* 상태 공간 트리

#### back tracking vs DFS
* prunning 가지치기
* 해를 찾을 수 있는 경우 중, 해를 찾을 수 없을것 같으면 해당 경로 중단
* 불필요한 경우의 수는 생략해서 속도가 빠름
* 최악의 경우는 여전히 지수함수 시간
* DFS는 모든 경우의 수 탐색
  
#### 미로 찾기
1. 현재 위치에서 갈 수 있는 경로 후보군 집합을 생성
2. 후보군 중 하나를 선택해서 진행
3. 더 이상 진행이 불가능하다면 돌아와서
4. 후보군의 다른 경로로 진행 반복

#### n-Queen
```python
# pseudo code
def checkNode(v):
    if promising(v):
        if solution at v:
            write solution
        else:
            for u in each child of v:
                checkNode(v)
```
* back tracking으로 풀이하면 확인하는 node수가 적어 속도가 빠름

### 부분집합
* powerset
* 어떤 집합의 공집합과 자기자신을 포함한 모든 부분집합
* 부분집합의 개수는 2^n
* True, False로 구성된 n개의 배열로 구현
>반복문으로 구현(1)
```python
bit = [0, 0, 0, 0]
for i in range(2):              # idx 0
    bit[0] = i
    for j in range(2):          # idx 1
        bit[1] = j
        for k in range(2):      # idx 2
            bit[2] = k
            for l in range(2):  # idx 3
                bit[3] = l
                print(bit)
```
>부분 집합 템플릿 1 , back tracking으로 구현
```python
# 후보군의 경우의 수를 탐색하여, 해를 찾기
def backTracking(a, k ,input):
    global max_candidates
    c = [0]*max_candidates

    if k == input:
        # solution
        # 원하는 해일 때, 필요한 작업 수행
        # process_solution(a, k)
        pass
    else:
        k += 1
        # 후보군을 생성해서, 그 후보군의 가능한 경우의 수만 탐색
        ncandidates = constructCandidates(a, k ,input, c)
        for i in range(ncandidates):
            a[k] = c[i]
            backTracking(a, k, input)

# 후보군을 생성하는 함수
def constructCandidates(a, k, input, c):
    c[0] = True
    c[1] = False
    return 2

max_candidates = 2
nmax = 4
a = [0]*nmax
backTracking(a, 0, 3)
```
>부분 집합 템플릿 2
```python
def powerset(idx):                  # 몇 번째 idx가 선택o / 선택x
    if idx < len(lst):              # 사용되는 숫자를 정할 수 있음
        selected[idx] = True
        powerset(idx+1)
        selected[idx] = False
        powerset(idx+1)
    else:
        # 부분 집합을 생성
        res = []
        for i in range(len(lst)):
            if selected[i]:
                res.append(lst[i])
        # print(res)
        result.append(res)

lst = list(range(1, 6))
selected = [False]*len(lst)
result = []

powerset(0)
print(result)
print(len(result))
```
>combinations 사용
```python
from itertools import combinations 

arr = [1, 2, 3]
result = []
for i in range(len(arr)+1):
  result += list(combinations(arr, i))  

print(result)
```

#### 예제
* {1, 2, 3, 4, 5, 6, 7, 8, 9, 10} 에서 합이 10인 부분집합 출력

(1) 백트래킹 가지치기
  * 부분 집합을 생성하면서 합을 계산하여, 10이 될 수 없는 경우의 수는 도중에 중단
  1. i원소의 포함 여부를 확인하면서, 부분 집합의 합 s 확인
  2. 합 s가 10보다 크면 중단
>(1) 가지치기
```python
def subsetSum(i, N, s, t):
    global result
    if i == N:                              # 모든 원소를 확인
        if s == t:                          # 부분집합의 합이 t이면
            result += 1
        return
    elif s > t:
        return
    else:
        subsetSum(i+1, N, s+A[i], t)        # A[i]가 포함된 경우 
        subsetSum(i+1, N, s, t)        # A[i]가 포함되지 않은 경우

A = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
bit = [0]*len(A)
result = 0

subsetSum(0, 10, 0, 10)
print(result)
```

(2) (1)보완하기
* 가지치기로 확인하는 node수는 줄어들지만,
* 찾으려는 부분 집합의 합이 큰 수라면 효과가 떨어짐
* 남은 원소의 합을 추가로 고려하기!
  * 남은 원소의 합을 다 더해도 찾으려는 값보다 작을 경우 중단
  * S + RS(rest_sum 남은 구간의 합) < T
>(2) 보완, 중단 조건 추가
```python
def subsetSum(i, N, s, t):
    global result, cnt
    cnt += 1
    rest_sum = sum(A[i:])
    if i == N:                              # 모든 원소를 확인
        if s == t:                          # 부분집합의 합이 t이면
            result += 1
        return
    elif s > t:
        return
    elif s + rest_sum < t:
        return
    else:
        subsetSum(i+1, N, s+A[i], t)        # A[i]가 포함된 경우 
        subsetSum(i+1, N, s, t)             # A[i]가 포함되지 않은 경우

A = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
bit = [0]*len(A)
result = 0
cnt = 0
subsetSum(0, 10, 0, 30)
print(result, cnt)
```
* 합이 30을 구하는 경우
* ![호출 횟수 비교](./img/2022-08-23-22-13-25.png)
* 1) 모든 경로 탐색 : 2047
* 2) s > t 조건 추가 : 1843
* 3) s + RS < t 까지 추가 : 1381

(3) 완전 검색
  * 혹은 모든 부분 집합 생성 후, 합이 10인 경우만 출력
>(3) 완전 검색
```python
def subsetSum(i, N):
    global result
    if i == N:
        # print(bit)
        temp = 0
        for i in range(N):
            if bit[i]:
                temp += A[i]
        #         print(A[i], end=' ')
        # print()
        if temp == 10:      # 합이 10이면 개수 추가
            result += 1
            for i in range(N):
                if bit[i]:
                    print(A[i], end=' ')
            print()
    else:
        bit[i] = 1          # A[i]가 부분집합에 포함된 경우
        subsetSum(i+1, N)
        bit[i] = 0          # A[i]가 부분집합에 포함되지 않은 경우
        subsetSum(i+1, N)

A = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
bit = [0]*len(A)
result = 0

subsetSum(0, 10)
print(result)
```

### 순열
>back tracking으로 구현
```python
# 후보군의 경우의 수를 탐색하여, 해를 찾기
def backTracking(a, k, input):
    global max_candidates
    c = [0]*max_candidates

    if k == input:
        # solution
        for i in range(1, k+1):
            print(a[i], end=' ')
        print()
    else:
        k += 1
        # 후보군을 뽑아서, 경우의 수 진행
        ncandidates = constructCandidates(a, k, input, c)
        for i in range(ncandidates):
            a[k] = c[i]
            backTracking(a, k, input)

# 후보군을 생성하는 함수
# 순열의 중복 x, 앞에서 사용하지 않은 수를 후보군으로 뽑기
def constructCandidates(a, k, input, c):
    in_perm = [False]*nmax

    for i in range(1, k):
        in_perm[a[i]] = True

    ncandidates = 0
    for i in range(1, input+1):
        if in_perm[i] == False:
            c[ncandidates] = i
            ncandidates += 1
    
    return ncandidates

max_candidates = 4
nmax = 4
a = [0]*nmax
backTracking(a, 0, 3)
```
* 추가 내용
>pseudo code
```python
f(i, N)
    if i == N               # 순열 완성

    else
        for j: i -> N-1     # 가능한 모든 원소에 대해
            P[i] <-> P[j]   # P[i] 결정
            f(i+1, N)
            P[i] <-> P[j]   # P[i] 복구
```
>순열 생성 템플릿
```python
def nPr(i, N):
    if i == N:                      # 순열 완성 
        result.append(P)
        print(P)
    else:
        for j in range(i, N):       # P[i]에 들어갈 P[j] 결정
            P[i], P[j] = P[j], P[i]
            nPr(i+1, N)
            P[i], P[j] = P[j], P[i]

result = []
P = [1, 2, 3, 4]
nPr(0, 4)
```
>permutation 사용
```python
from itertools import permutations

r = 2
arr = [1, 2, 3, 4]
nPr = list(permutations(arr, r))

print(nPr)
```
>반복문으로 구현
```python
arr = [1, 2, 3]
subsets = [[]]

for num in arr:
    size = len(subsets)
    for i in range(size):
        subsets.append(subsets[i] + [num])
        
print(subsets)
```
* 순서대로
* [1, 2, 3], [2, 1, 3], [3, 2, 1]
* [1, 3, 2]
* 순열을 생성한 이후, 각 경우별로 cost를 계산할 때 사용!
---
title: "백트래킹"
description: "백트래킹 Backtracking 알고리즘"
date: 2025-02-11 10:00:00 +0900
categories: [CS, Algorithm]
tags: [CS, Algorithm, bubble sort, backtracking]
---

## **🔍 백트래킹(Backtracking) 개념 정리**
백트래킹(Backtracking)은 **모든 경우를 탐색하면서 불필요한 경로는 조기에 중단(Pruning)하는 알고리즘 기법**이다.  
즉, 가능한 모든 경우를 시도하지만, **현재 경로가 답이 될 가능성이 없으면 더 깊이 탐색하지 않고 되돌아간다.**  

✅ **완전 탐색(Brute Force)보다 효율적이며, 최적의 해를 찾는 문제에 자주 사용된다.**  
✅ **DFS(깊이 우선 탐색)과 함께 사용되며, 특정 조건을 만족하는 경우에만 탐색을 진행한다.**

---

## **1. 백트래킹의 핵심 원리**
1. **현재 상태가 유망한지(가능성이 있는지) 검사한다.**  
   - 가능성이 있다면 탐색을 진행한다.
   - 가능성이 없다면 해당 경로를 더 이상 탐색하지 않고 되돌아간다. (Pruning)

2. **탐색이 끝나면 원래 상태로 되돌려야 한다.**  
   - 특정 선택을 했을 때, 이후 경로를 탐색한 후 다시 이전 상태로 복구해야 한다. (Backtrack)

3. **DFS(깊이 우선 탐색)과 함께 사용된다.**  
   - 상태 공간을 탐색하면서 해를 찾는 과정에서 **백트래킹을 활용해 탐색을 줄인다.**

---

## **2. 백트래킹을 사용하는 경우**
✅ **모든 경우의 수를 탐색해야 하지만, 불필요한 탐색을 줄여야 할 때**  
✅ **"최적의 해" 또는 "특정 조건을 만족하는 해"를 찾아야 할 때**  
✅ **가능한 경우의 수가 많아 `N!`, `2^N`과 같은 조합을 탐색해야 하는 경우**

예를 들어, `N!` 또는 `2^N` 개의 경우를 모두 탐색하는 문제에서 **불가능한 경우를 미리 제외**하면 탐색을 크게 줄일 수 있다.

---

## **3. 백트래킹의 동작 방식**
1. **DFS(깊이 우선 탐색)으로 탐색을 시작**한다.
2. **현재 상태가 정답이 될 가능성이 없으면 탐색을 중단(Pruning).**
3. **가능성이 있다면 다음 단계로 탐색을 진행한다.**
4. **탐색이 끝나면 이전 단계로 돌아가서 다른 경우를 탐색한다. (Backtrack)**

---

## **4. 백트래킹을 활용한 예제 문제**
### **(1) N-Queen 문제 (8×8 체스판에 퀸을 놓는 경우)**
✅ `N×N` 체스판에서 `N`개의 퀸을 서로 공격하지 않도록 놓는 방법의 개수를 구하는 문제  
✅ 퀸은 같은 행, 같은 열, 대각선 방향으로 공격할 수 있음  

```python
def is_safe(board, row, col, n):
    for i in range(row):
        if board[i] == col or abs(board[i] - col) == row - i:
            return False
    return True

def solve_n_queens(n, row=0, board=[]):
    if row == n:
        print(board)  # 가능한 퀸 배치 출력
        return 1

    count = 0
    for col in range(n):
        if is_safe(board, row, col, n):
            count += solve_n_queens(n, row + 1, board + [col])  # 다음 행 탐색
    return count

print(solve_n_queens(4))  # 2 (4×4 체스판에 2가지 경우 가능)
```
✅ **가능성이 없는 경우(퀸이 공격받는 경우) 즉시 탐색을 중단하여 효율적으로 해결**

---

### **(2) 순열 구하기 (모든 경우 탐색)**
✅ 중복 없이 순열을 구하는 백트래킹 예제  

```python
def backtrack(arr, path, used, result):
    if len(path) == len(arr):
        result.append(path[:])  # 순열 완성 시 저장
        return

    for i in range(len(arr)):
        if not used[i]:
            used[i] = True
            path.append(arr[i])
            backtrack(arr, path, used, result)
            path.pop()  # 백트래킹 (이전 단계로 되돌아감)
            used[i] = False

def permutations(arr):
    result = []
    backtrack(arr, [], [False] * len(arr), result)
    return result

print(permutations([1, 2, 3]))
```
✅ **불가능한 경우를 미리 배제하여 중복된 탐색을 방지**

---

## **5. 백트래킹을 활용한 던전 탐험 문제**
이전 문제에서 **순열(모든 경우의 수 탐색)**을 사용했지만, 백트래킹을 활용하여 더 효율적으로 풀 수도 있다.

```python
def dfs(k, dungeons, visited, count):
    global max_dungeons
    max_dungeons = max(max_dungeons, count)  # 최대 탐험 가능 던전 개수 업데이트

    for i in range(len(dungeons)):
        if not visited[i] and k >= dungeons[i][0]:  # 탐험 가능하면
            visited[i] = True
            dfs(k - dungeons[i][1], dungeons, visited, count + 1)  # 피로도 차감 후 탐색
            visited[i] = False  # 백트래킹

def solution(k, dungeons):
    global max_dungeons
    max_dungeons = 0
    visited = [False] * len(dungeons)  # 방문 여부 체크 배열
    dfs(k, dungeons, visited, 0)  # DFS 탐색 시작
    answer = max_dungeons
    return answer
```
✅ **불가능한 경우(피로도가 부족한 경우) 즉시 가지치기(Pruning)하여 효율적인 탐색 가능**

---

## **6. 백트래킹 vs 완전 탐색 비교**
| 방식 | 탐색 방법 | 가지치기 | 성능 |
|---|---|---|---|
| 완전 탐색 (Brute Force) | 모든 경우 확인 | ❌ 없음 | 느림 (`O(N!)` 또는 `O(2^N)`) |
| 백트래킹 (Backtracking) | 가능한 경우만 확인 | ✅ 있음 | 빠름 (`O(N!)` 또는 `O(2^N)`에서 많이 감소) |

---

## **📌 결론**
✅ **백트래킹은 가능한 모든 경우를 탐색하되, 불필요한 경우를 미리 제외(Pruning)하여 효율성을 높이는 기법이다.**  
✅ **DFS(깊이 우선 탐색)과 함께 자주 사용되며, 최적의 해를 찾거나 특정 조건을 만족하는 경우를 찾는 데 유용하다.**  
✅ **순열, 조합, N-Queen, 던전 탐험, 경로 찾기 등 다양한 문제에서 활용할 수 있다.**  

❌ **완전 탐색보다 효율적이며, 필요 없는 경로를 미리 배제하는 것이 핵심이다.**  
✅ **"되돌아가면서 탐색을 줄이는 완전 탐색"이 바로 백트래킹이다!**
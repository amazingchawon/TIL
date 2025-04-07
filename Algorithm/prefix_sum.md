# Prefix Sum (누적합)

> 배열에서 특정 구간의 합을 **빠르게 계산**하기 위해  
> **지금까지의 합을 미리 계산해 저장**해두는 기법입니다.

---

## 📌 1차원 누적합

### 핵심 아이디어

- `prefix[i] = arr[0] + arr[1] + ... + arr[i]`
- 구간 합: `arr[l] + ... + arr[r] = prefix[r] - prefix[l - 1]`

### 예제 코드

```python
arr = [1, 2, 3, 4]
prefix = [0] * len(arr)

prefix[0] = arr[0]
for i in range(1, len(arr)):
    prefix[i] = prefix[i - 1] + arr[i]

# 예: 1~3번 인덱스 구간합 = prefix[3] - prefix[0] = 10 - 1 = 9
```

---

## 📌 2차원 누적합

### 인덱스 기준

- `arr[x][y]` → `x`는 행, `y`는 열
- `S[x][y]`는 (0,0)부터 (x,y)까지의 직사각형 합

### 누적합 공식

```python
S[x][y] = arr[x][y]
        + S[x-1][y]
        + S[x][y-1]
        - S[x-1][y-1]
```

> ⚠️ 경계 조건 (x==0 또는 y==0)일 때는 인덱스 에러 주의

---

### 예제 코드

```python
N = 3
arr = [
    [1, 2, 3],  # x = 0
    [4, 5, 6],  # x = 1
    [7, 8, 9]   # x = 2
]

# 누적합 테이블 (0-based)
S = [[0] * N for _ in range(N)]

for x in range(N):
    for y in range(N):
        S[x][y] = arr[x][y]  # 현재 위치 값부터 시작

        # 위쪽에서 내려온 누적합 추가
        if x > 0:
            S[x][y] += S[x-1][y]

        # 왼쪽에서 누적합 추가
        if y > 0:
            S[x][y] += S[x][y-1]

        # 위 + 왼쪽이 겹치는 부분을 중복 제거
        if x > 0 and y > 0:
            S[x][y] -= S[x-1][y-1]

```

---

### 구간합 구하기

> (x1, y1)부터 (x2, y2)까지 직사각형 합 구하기

```python
def get_sum(x1, y1, x2, y2):
    res = S[x2][y2]
    if x1 > 0:
        res -= S[x1-1][y2]
    if y1 > 0:
        res -= S[x2][y1-1]
    if x1 > 0 and y1 > 0:
        res += S[x1-1][y1-1]
    return res
```

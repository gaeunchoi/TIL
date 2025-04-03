# 동적 프로그래밍 Dynamic Programming, DP

> 복잡한 문제를 **간단한 여러 개의 문제로 나누어 푸는** 방식

<br>

## 💡 아이디어

### 중복 하위 문제 Overlapping Subproblems

문제를 해결하는 과정에서 동일한 하위 문제가 반복적으로 계산되는 경우를 의미

ex) 피보나치 수열을 단순 재귀로 구현할 때 동일한 부분 문제가 여러 번 호출

### 최적 부분 구조 Optimal Substructure

**작은 부분 문제에서 구한 최적해**로 합쳐진 **큰 문제의 최적해**를 구할수 있어야 하는것을 의미

ex) 피보나치 수열에서 `F(n) = F(n-1) + F(n-2` 로 전체 문제를 작은 문제들로 분할하여 해결

여기서, 중복되는 연산 과정을 줄이기 위해 메모이제이션 개념이 필요하다.

### 메모이제이션 Memoization

위에서 언급한 같은 계산을 반복하지 않기 위해 이미 계산한 값을 저장해서 중복 계산을 줄이는 것을 의미

큰 문제를 작은 하위문제로 나누었을때, 하나의 문제는 단 한번만 풀도록 하는 알고리즘

<br>

## 🚀 접근방식

### 탑다운 Top-Down

위에서 아래로 접근하는 방식으로, 큰 문제에서 부분 문제로 쪼개가며 재귀호출을 통해 문제를 해결

### 바텀업 Button-Up

아래서 위로 접근하는 방법으로, 부분 문제에서부터 문제를 해결하여 점차 큰 문제를 해결

<br>

## 💻 구현 예시

### 피보나치 수열의 Top-Down Approach

```jsx
function fib(n, memo = {}) {
  if (n <= 1) return n;
  if (n in memo) return memo[n];

  memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
  return memo[n];
}

console.log(fib(10)); // 55
```

### 피보나치 수열의 Button-UP Approach

```jsx
function fib(n) {
  if (n <= 1) return n;
  const dp = [0, 1];

  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }

  return dp[n];
}

console.log(fib(10)); // 55
```

<br>

## ✅ 장점 / 단점

### 장점

- 중복 계산을 줄여 효율성 향상
- 다양한 문제에 응용 가능(경로 찾기, 배낭 문제 등)

### 단점

- 메모리 사용량 증가(메모이제이션)
- 문제의 구조를 잘 분석해야함(점화식 찾기)

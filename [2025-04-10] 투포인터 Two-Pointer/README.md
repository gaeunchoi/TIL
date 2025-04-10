## 투포인터 Two-Pointer

> 배열이나 리스트에서 두 개의 포인터(변수, 인덱스)를 사용해서 문제를 해결하는 알고리즘

- 포인터 두 개가 왼쪽에서 오른쪽으로 함께 이동
- 하나는 앞에서부터, 다른 하나는 뒤에서부터 이동
- 슬라이딩 윈도우처럼 일정 범위의 합을 구할때도 활용

### 사용 예시

- 정렬된 배열에서 조건을 만족하는 쌍 찾기 → 특정 합을 만드는 두 수 찾기
- 특정 범위의 누적합 구하기 → 연속된 부분합, 최대 길이 부분 배열
- 중복 없이 구간 내 탐색 → 슬라이딩 윈도우, 문자열 조합

### 아이디어

1. 두 개의 포인터를 이용해 한 번의 순회로 해결한다

   → 이중 반복문보다 훨씬 빠르게 동작

2. 불필요한 반복을 줄여 효율성 상승
3. 시간 복잡도를 O($n^2$)에서 O($n$) 또는 O($n log n$)으로 개선

### 사용 예시 - 정렬된 배열에서 두 수의 합 찾기

정렬된 배열에서 두 수의 합이 target이 되는 두 수의 인덱스를 구하는 예시

배열이 오름차순 정렬되어 있다는 점을 이용

- 두 수의 합이 목표보다 작으면 → 왼쪽 포인터 증가
- 두 수의 합이 목표보다 크면 → 오른쪽 포인터 감소

```jsx
function twoSum(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    const sum = arr[left] + arr[right];

    if (sum === target) return [left, right];
    if (sum < target) left++;
    else right--;
  }

  return [];
}

console.log(twoSum([1, 2, 3, 4, 6], 6));
// [1, 3] => 2 + 4 = 6
```

### 사용 예시 - 연속된 부분합의 길이(슬라이딩 윈도우)

최소 부분합이 S 이상인 가장 짧은 연속 부분 배열의 길이를 구하는 예시

- `right`가 오른쪽으로 이동하면서 `sum`을 누적
- sum ≥ S가 되면 left를 오른쪽으로 밀면서 최소 길이를 계산

```jsx
function minSubArrayLen(S, arr) {
  let left = 0,
    right = 0;
  let sum = 0;
  let minLen = Infinity;

  while (right < arr.length) {
    sum += arr[right++];

    while (sum >= S) {
      minLen = Math.min(minLen, right - left);
      sum -= arr[left++];
    }
  }

  return minLen === Infinity ? 0 : minLen;
}

console.log(minSubArrayLen(7, [2, 3, 1, 2, 4, 3]));
// 2 → [4, 3]
```

### 시간복잡도

| 상황                | 시간 복잡도               |
| ------------------- | ------------------------- |
| 일반적인 투 포인터  | O(n)                      |
| 중첩 루프 없는 경우 | 매우 효율적 (대부분 O(n)) |

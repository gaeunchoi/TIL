## 📌 합병 정렬 Merge Sort

> **분할 정복 (Divide and Conquer)** 기법을 사용하여 리스트를 두 부분으로 나누고, 각각을 재귀적으로 정렬한 뒤 병합하여 완성하는 정렬 알고리즘

### 장점

- 안정 정렬(Stable Sort)이다
- 항상 O($n log n$)의 시간복잡도를 보장한다

### 단점

- 추가적인 배열 공간이 필요해 메모리 사용량이 크다

### 동작 과정

1. 주어진 리스트를 **반으로 나눔**
2. 각 부분을 **재귀적으로 정렬**
3. 두 정렬된 리스트를 **병합하여 하나의 정렬된 리스트로 만듦**

### 코드

```jsx
function mergeSort(arr) {
  if (arr.length <= 1) return arr;

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
}

function merge(left, right) {
  const result = [];
  let i = 0,
    j = 0;

  while (i < left.length && j < right.length) {
    if (left[i] < right[j]) result.push(left[i++]);
    else result.push(right[j++]);
  }

  return result.concat(left.slice(i)).concat(right.slice(j));
}

console.log(mergeSort([5, 3, 8, 4, 2])); // [2, 3, 4, 5, 8]
```

### 시간/공간 복잡도

- 시간 복잡도: O($n log n$) -> 모든 경우에서 동일
- 공간 복잡도: O($n$) → 추가 배열 사용

<br>

## 📌 퀵 정렬 Quick Sort

> **분할 정복 (Divide and Conquer)** 기법을 사용하여 리스트를 특정 기준(Pivot)을 기준으로 두 부분으로 나누고 재귀적으로 정렬하는 알고리즘

### 장점

- 추가적인 메모리 사용이 적다
- 평균적으로 매우 빠르다

### 단점

- 최악의 경우 O($n^2$)까지 성능이 저하될 수 있다
- 불안정 정렬(Stable Sort가 아니다)

### 동작 과정

1. **피벗(Pivot)**을 설정
2. 피벗보다 작은 값은 왼쪽, 큰 값은 오른쪽으로 분할
3. 각각의 부분 배열에 대해 재귀적으로 정렬

### 코드

```jsx
function quickSort(arr) {
  if (arr.length <= 1) return arr;

  const pivot = arr[Math.floor(arr.length / 2)];
  const left = arr.filter((x) => x < pivot);
  const right = arr.filter((x) => x > pivot);
  const middle = arr.filter((x) => x === pivot);

  return [...quickSort(left), ...middle, ...quickSort(right)];
}

console.log(quickSort([5, 3, 8, 4, 2])); // [2, 3, 4, 5, 8]
```

### 시간/공간 복잡도

- 시간 복잡도(평균, 최선): O($n log n$)
- 시간 복잡도(최악): O($n^2$) → 이미 정렬된 배열에서 피벗을 잘못 선택하는 경우
- 공간 복잡도: O($log n$)→ 재귀 호출로 인한 스택 메모리 사용

<br>

## **📌 계수 정렬 Counting Sort**

> **정수나 정수로 표현 가능한 데이터**를 정렬할 때 사용되는 알고리즘, 각 값의 발생 빈도를 계산하여 정렬

### 장점

- 매우 빠르며, O(n) 시간 복잡도를 가짐
- 특정 범위의 정수 정렬에 효율적

### 단점

- 값의 범위가 클 경우, 메모리 사용량 증가
- 비교 기반 정렬이 아니라, 범용적 사용 어려움

### 동작 과정

1. 입력 리스트의 각 값을 **카운팅 배열에 저장**
2. 카운팅 배열을 순회하며 **정렬된 리스트를 생성**

### 코드

```jsx
function countingSort(arr, maxValue) {
  const count = new Array(maxValue + 1).fill(0);
  const result = [];

  arr.forEach((num) => count[num]++);

  count.forEach((frequency, num) => {
    for (let i = 0; i < frequency; i++) {
      result.push(num);
    }
  });

  return result;
}

console.log(countingSort([5, 3, 8, 4, 2], 8)); // [2, 3, 4, 5, 8]
```

### 시간/공간 복잡도

- 시간 복잡도: O($n+k$) → $k$는 값의 범위
- 공간 복잡도: O($k$) → 값의 범위에 비례

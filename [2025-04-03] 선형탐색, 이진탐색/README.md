## 📌 선형 탐색 (Linear Search)

> 리스트의 **첫 번째 원소부터 마지막 원소까지 하나씩 순차적으로** 탐색

### 장점

- 정렬되지 않은 배열에서도 사용 가능
- 구현이 매우 간단

### 단점

- 데이터가 많을 경우 비효율적

### 동작 과정

1. 리스트의 첫 번째 원소부터 순서대로 비교.
2. 원하는 값을 찾으면 탐색 종료, 아니면 다음 원소로 이동.
3. 끝까지 찾아도 없다면 탐색 실패.

### 코드

```jsx
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return i; // 값이 발견되면 인덱스 반환
  }
  return -1; // 값이 없으면 -1 반환
}

console.log(linearSearch([4, 2, 9, 7, 1], 7)); // 3
```

### 시간 복잡도

- 최선의 경우: O(1)
- 평균/최악의 경우: O(n)

<br>

## 📌 이진 탐색 (Binary Search)

### 정의

> 정렬된 리스트에서 **중간 값을 기준으로 절반씩 탐색 범위를 줄여나가**는 방법

### 장점

- 탐색 속도가 빠름 (특히 데이터 크기가 큰 경우)
- 시간 복잡도가 **O($log n$)** 으로 효율적

### 단점

- 리스트가 **정렬되어 있어야 함**

### 동작 과정

1. 리스트의 중간 값을 확인.
2. 찾는 값이 중간 값보다 작으면 왼쪽 부분 탐색, 크면 오른쪽 부분 탐색.
3. 값을 찾거나 탐색 범위가 없을 때까지 반복.

### 코드

```jsx
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }

  return -1;
}

console.log(binarySearch([1, 2, 4, 7, 9], 7)); // 3
```

### 시간 복잡도

- **최선의 경우:** O(1)
- **평균/최악의 경우:** O(log n)

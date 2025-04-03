## 📌 선택 정렬 Selection Sort

> 가장 작은 값을 찾아 맨 앞의 요소와 교환하는 과정을 반복하여 정렬한다.

### 장점

- 구현이 간단하다
- 메모리 소비가 적다
- 이미 정렬된 배열에 대해서도 일정하게 작동한다

### 단점

- 시간 복잡도가 O($n^2$)으로 느리다
- 데이터 이동(교환) 횟수가 많다
- 데이터 양이 많은 경우 비효율적이다

### 동작 과정

1. **가장 작은 값을 찾음**
2. 찾은 값을 **현재 위치와 교환**
3. 리스트의 끝까지 반복하여 정렬

### 코드

```jsx
function selectionSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    let minIndex = i;
    for (let j = i + 1; j < n; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }
  return arr;
}

console.log(selectionSort([5, 3, 8, 4, 2])); // [2, 3, 4, 5, 8]
```

### 시간/공간 복잡도

- 시간 복잡도: O($n^2$) → 모든 경우에 동일
- 공간 복잡도: O(1) → 추가 메모리 사용 x

<br>

## 📌 버블 정렬 Bubble Sort

> 인접한 두 요소를 비교하여 큰 값을 뒤로 보내는 방식을 반복하여 정렬한다.

### 장점

- 구현이 쉽다
- 배열이 거의 정렬된 경우 빠르게 종료된다
- 안정 정렬로, 같은 값의 순서가 유지된다

### 단점

- 시간 복잡도가 O($n^2$)로 비효율적이다
- 특히, 데이터가 역순일 경우 성능이 저하된다
- 비교 및 교환이 많이 발생한다

### 동작 과정

1. **인접한 두 요소를 비교하여 교환**
2. 큰 값이 뒤로 밀려가며 정렬
3. 리스트가 완전히 정렬될 때까지 반복

### 코드

```jsx
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    for (let j = 0; j < n - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return arr;
}

console.log(bubbleSort([5, 3, 8, 4, 2])); // [2, 3, 4, 5, 8]
```

### 시간/공간 복잡도

- 시간 복잡도(최악, 평균): O($n^2$)
- 시간 복잡도(최선): O(n) → 이미 정렬된 경우
- 공간 복잡도: O(n) → 추가 메모리 사용 x

<br>

## 📌 삽입 정렬 Insertion Sort

> 정렬된 부분을 유지하며 새로운 값을 적절한 위치에 삽입하여 정렬한다.

### 장점

- 거의 정렬된 배열에서 매우 빠르다
- 안정 정렬이다
- 추가적인 메모리 사용이 거의 없다

### 단점

- 시간 복잡도가 O($n^2$)으로 큰 데이터 집합에서 비효율적이다
- 삽입 위치를 찾기 위해 많은 비교가 필요할 수 있다

### 동작 과정

1. 두 번째 요소부터 시작하여 **정렬된 부분과 비교하여 삽입 위치를 찾음**
2. 적절한 위치에 삽입
3. 리스트의 끝까지 반복

### 코드

```jsx
function insertionSort(arr) {
  const n = arr.length;
  for (let i = 1; i < n; i++) {
    let current = arr[i];
    let j = i - 1;
    while (j >= 0 && arr[j] > current) {
      arr[j + 1] = arr[j];
      j--;
    }
    arr[j + 1] = current;
  }
  return arr;
}

console.log(insertionSort([5, 3, 8, 4, 2])); // [2, 3, 4, 5, 8]
```

### 시간/공간 복잡도

- 시간 복잡도(최악, 평균): O($n^2$)
- 시간 복잡도(최선): O(n) → 거의 정렬된 경우
- 공간 복잡도: O(1) → 추가 메모리 사용 x

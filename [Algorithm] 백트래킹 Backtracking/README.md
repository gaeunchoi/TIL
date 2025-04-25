## 백트래킹 Backtracking

> 모든 경우의 수를 탐색하되, 조건에 맞지 않으면 되돌아가서 다른 경로를 탐색하는 알고리즘

- 가능성이 없는 경로는 미리 포기하고 더 깊이 탐색하지 않는 방식(가지치기)
- 모든 경우를 다 탐색하는 브루트포스(완전 탐색)에서 불필요한 탐색을 피하기 때문에 효율적

### 가지치기

불필요한 탐색을 미리 중단하는 것을 의미한다. 이는 성능 향상과 직결되어있다.

예를들어, 합이 목표를 넘으면 더이상 탐색하지 않거나, 이미 방문한 노드는 다시 방문하지 않게 조건을 거는 것이다.

### 기본 구조

```jsx
function backTracking(path, options){
	if(종료 조건){
		결과.push([...path]);
		return;
	}

	for(let i = 0 ; i < options.length ; i++){
		if(가지치기 조건) continue;
		path.push(options[i]);
		backTracking(path, options);
		path.pop();
	}
}
```

### 예시

1부터 n까지 숫자 중 k개를 고르는 조합

```jsx
function combination(n, k) {
  const result = [];

  function backtracking(start, path) {
    if (path.length === k) {
      result.push([...path]);
      return;
    }

    for (let i = start; i <= n; i++) {
      path.push(i);
      backtracking(i + 1, path);
      path.pop();
    }
  }

  backtracking(1, []);
  return result;
}

console.log(combination(4, 2));
// [[1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4]]
```

### 백트래킹 vs. 브루트포스

| 항목 | 브루트포스        | 백트래킹                |
| ---- | ----------------- | ----------------------- |
| 방식 | 모든 경우 탐색    | 조건에 따라 탐색 생략   |
| 성능 | 느림 (O(n!))      | 빠름 (가지치기 효과)    |
| 예시 | 모든 조합 다 탐색 | 조건에 맞는 조합만 탐색 |

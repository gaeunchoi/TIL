# 📌 우선순위 큐 Priority Queue

> **우선순위(priority)가 높은 요소가 먼저 나오는 큐** 형태의 자료구조

일반적인 큐는 먼저 들어온 요소가 먼저 나가는 FIFO(First-In First-Out) 구조([큐 정리 블로그](https://velog.io/@gaeunchoi/Algorithm-%EC%8A%A4%ED%83%9D%EA%B3%BC-%ED%81%90))이나, 우선순위 큐는 들어온 순서보다 우선순위에 따라 요소가 처리된다.

### 사용 예시

- [다익스트라 알고리즘 Dijkstra Algorithm](https://velog.io/@gaeunchoi/Algorithm-%EC%B5%9C%EB%8B%A8%EA%B2%BD%EB%A1%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Dijkstra-Bellman-Ford)
- [힙 정렬](https://velog.io/@gaeunchoi/Algorithm-%ED%9E%99Heap%EA%B3%BC-%ED%9E%99%EC%A0%95%EB%A0%ACHeap-Sort)
- 운영체제의 작업 스케쥴링 등 ,,

### 구현 방법

- 배열: 삽입은 빠르지만, 삭제시 우선순위 탐색이 느림(O(n))
- 힙 Heap: 삽입과 삭제 모두 O(log n)으로 효율적

보통 **우선순위큐는 힙으로 구현**한다.

### 우선순위큐 구현 코드(Min Heap)

```jsx
class PriorityQueue {
	constructor() {
		this.heap = [];
	}

	enqueue(value, prio){
		this.heap.push({val, prio});
		this._bubbleUp();
	}

	dequeue(){
		const min = this.heap[0];
		const end = this.heap.pop();
		if(this.heap.length > 0){
			this.heap[0] = end;
			this._sinkDown();
		}
		return min;
	}

	_bubbleUp(){
		let idx = this.heap.length - 1;
		const element = this.heap[idx];

		while(idx > 0){
			let parentIdx = Math.floor((idx-1) / 2);
			let parent = this.heap[parentIdx];

			if(element.prio >= parent.prio) break;

			this.heap[parentIdx] = element;
			this.heap[idx] = parent;
			idx = parentIdx;
		}
	}

	_sinkDown() {
		let idx = 0;
		const length = this.heap.length;
		const element = this.heap[0];

		while(true){
			let leftIdx = 2 * idx + 1;
			let rightIdx = 2 * idx + 2;
			let swap = null;

			if(leftIdx < length){
				let left = this.heap[leftIdx];
				if(left.prio < element.prio) swap = leftIdx;
			}

			if(rightIdx < length){
				let right = this.heap[rightIdx];
				if(swap === null && right.prio < element.prio) || (swap !== null && right<prio < this.heap[swap].prio)) swap = rightIdx;
			}

			if(swap === null) break;

			this.heap[idx] = this.heap[swap];
			this.heap[swap] = element;
			idx = swap;
		}
	}
}

const result = new PriorityQueue();
result.enqueue('Clean room', 3);
result.enqueue('Do homework', 1);
result.enqueue('Play game', 2);

console.log(result.dequeue());
// { value: 'Do homework', priority: 1 }

console.log(result.dequeue());
// { value: 'Play game', priority: 2 }
```

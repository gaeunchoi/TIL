# **📌 이진 탐색 트리 Binary Search Tree**

> 각 노드가 최대 두 개의 자식 노드를 가지는 이진 트리

## 특징

- 트리의 왼쪽 서브트리에 있는 모든 값은 루트 노드의 값보다 작다
- 트리의 오른쪽 서브트리에 있는 모든 값은 루트 노드의 값보다 크다
- 각 서브트리 또한 이진 탐색 트리의 성질을 만족한다

## 장점

- 검색, 삽입, 삭제 연산의 시간복잡도가 평균적으로 O(log n)으로, 효율적이다
- 정렬된 데이터를 효과적으로 관리할 수 있다

## 단점

- 트리가 한쪽으로 치우쳐질 경우 성능이 저하될 수 있다

## 코드

```jsx
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  // 삽입
  insert(value) {
    const newNode = new TreeNode(value);
    if (!this.root) {
      this.root = newNode;
      return this;
    }

    let current = this.root;
    while (true) {
      if (value < current.value) {
        if (!current.left) {
          current.left = newNode;
          return this;
        }
        current = current.left;
      } else {
        if (!current.right) {
          current.right = newNode;
          return this;
        }
        current = current.right;
      }
    }
  }

  // 검색
  search(value) {
    let current = this.root;
    while (current) {
      if (value === current.value) return true;
      current = value < current.value ? current.left : current.right;
    }
    return false;
  }

  // 삭제
  delete(value) {
    this.root = this._deleteNode(this.root, value);
  }

  _deleteNode(node, value) {
    if (!node) return null;

    if (value < node.value) {
      node.lfet = this._deleteNode(node.left, value);
    } else if (value > node.value) {
      node.right = this._deleteNode(node.right, value);
    } else {
      if (!node.left && !node.right) return null;

      if (!node.left) return node.right;
      if (!node.right) return node.left;

      let minNode = this._findMin(node.right);
      node.value = minNode.value;
      node.right = this._deleteNode(node.right, minNode.value);
    }
    return node;
  }

  _findMin(node) {
    while (node.left) node = node.left;
    return node;
  }
}

const bst = new BinarySearchTree();
bst.insert(8);
bst.insert(3);
bst.insert(10);
bst.insert(1);
bst.insert(6);
bst.insert(14);
bst.insert(4);
bst.insert(7);
bst.insert(13);

console.log(bst.search(6)); // true (값이 존재)
console.log(bst.search(2)); // false (값이 없음)

bst.delete(3); // 3 삭제
console.log(bst.search(3)); // false (삭제됨)
```

## 시간 복잡도

| 연산 | 평균 시간 복잡도 | 최악 시간 복잡도 |
| ---- | ---------------- | ---------------- |
| 삽입 | O(log n)         | O(n)             |
| 검색 | O(log n)         | O(n)             |
| 삭제 | O(log n)         | O(n)             |

최악의 경우는 트리가 한쪽으로 치우쳐진 경우이다. 예를 들어 정렬된 데이터를 삽입할 때가 있다.

## 이진 탐색 vs 이진 검색 트리

- [이진 탐색 Binary Search](<[https://velog.io/@gaeunchoi/Algorithm-선형-탐색과-이진-탐색-xv1h6esg](https://velog.io/@gaeunchoi/Algorithm-%EC%84%A0%ED%98%95-%ED%83%90%EC%83%89%EA%B3%BC-%EC%9D%B4%EC%A7%84-%ED%83%90%EC%83%89-xv1h6esg)>)
  - 탐색 대상이 **배열**이다
  - 반드시 정렬된 상태여야 한다
  - 정렬된 배열에서 특정 값을 빠르게 찾는 알고리즘이다
  - 배열의 중간값을 기준으로 탐색 범위를 절반씩 줄여나간다
- 이진 검색 트리 Binary Search Tree
  - 탐색 대상이 **트리**이다
  - 이진 트리의 한 종류이다
  - 왼쪽 자식 < 부모 < 오른쪽 자식의 크기 규칙을 따른다
  - 탐색 뿐만 아니라 삽입, 삭제도 빠르다

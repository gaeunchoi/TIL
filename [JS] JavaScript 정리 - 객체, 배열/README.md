# JavaScript 정리 - 3. 객체 & 배열

## 1️⃣ 객체

### 1. 정의

자바스크립트의 객체는 `키 key + 값 value`로 구성된 프로퍼티들의 집합이다.

### 2. 객체 생성

```jsx
// 빈 객체를 만드는 법
let user = new Object(); // 객체 생성자 문법
let user = {}; // 객체 리터럴 문법 (권장)
```

`객체 생성자`: new 키워드와 함께 객체를 생성하고 초기화하는 함수  
`객체 리터럴`: 중괄호 {…}를 이용해 객체를 선언하는 것

객체를 만들 때 `단축 프로퍼티`로 줄여서 쓸 수도 있다.

```jsx
function createUserInfo(name, age) {
  return {
    name: name, // 이렇게 키, 값 이름이 변수의 이름과 동일할 때 사용할 수 있는 단축 프로퍼티
    age: age,
  };
}

const user = createUserInfo("Lisa", 30);
console.log(user);

// 단축 프로퍼티로 수정해보면
function createUserInfo(name, age) {
  return {
    name, // name: name 과 같은 의미 입니다.
    age,
  };
}

const user = createUserInfo("Lisa", 30);
console.log(user);
```

### 3. in, for…in

- `in` 연산자: 프로퍼티 존재 여부 확인

```jsx
// example
const user = { name: "Lisa", age: 30 };
console.log("age" in user); // true
console.log("city" in user); // false
```

- `for ... in`: 객체의 모든 키를 순회

```jsx
for (key in object) {
  // 각 프로퍼티 키(key)를 이용하여 본문(body)을 실행합니다.
}

let user = {
  name: "John",
  age: 30,
  isAdmin: true,
};

for (let key in user) {
  // 키
  console.log(key); // name, age, isAdmin
  // 키에 해당하는 값
  console.log(user[key]); // John, 30, true
}
```

### 4. 객체 정렬

객체 프로퍼티(`키 key`)가 정수 프로퍼티이면 자동으로 정렬되고, 그 외의 프로퍼티는 객체에 추가한 순서 그대로 정렬된다.

```jsx
let codes = {
  49: "독일",
  41: "스위스",
  44: "영국",
  1: "미국",
};

for (let code in codes) {
  console.log(code); // 1, 41, 44, 49
}

let userInfos = {
  lisa: "lisa",
  john: "john",
  bob: "bob",
  mandoo: "mandoo",
};

for (let userInfo in userInfos) {
  console.log(userInfo); // lisa, john, bob, mandoo
}

// 만약, 숫자를 쓰고 싶은데 자동 정렬을 막고 싶다면 + 를 붙여 문자열인척 하자!
let codes = {
  "+49": "독일",
  "+41": "스위스",
  "+44": "영국",
  // ..,
  "+1": "미국",
};

for (let code in codes) {
  console.log(+code); // 49, 41, 44, 1
}
```

<br>

## 2️⃣ 배열

### 1. 정의

한 개의 변수에 여러 개의 값을 순차적으로 저장할 때 사용된다.

### 2. 배열 생성

아래 코드와 같이 배열을 생성할 수 있다.

```jsx
// 빈 배열은 이렇게 만들 수 있어요
let arr = new Array();
let arr = []; // 권장

// 배열 선언할 때 초기값 넣어주고 시작하는 것 가능함
let name = ["lisa", "mandoo", "john"];
```

### 3. 배열 요소 접근

- 인덱스를 이용하여 접근

```jsx
let name = ["lisa", "mandoo", "john"];
console.log(name[0]); // lisa
```

- 전체 배열에 접근

```jsx
let name = ["lisa", "mandoo", "john"];
document.getElementById("test").innerHTML = name;
```

- 인덱스를 이용하여 배열 해당 인덱스에 있는 요소 수정

```jsx
let name = ["lisa", "mandoo", "john"];
name[1] = "gaanii";
```

### 4. 배열의 속성 length

`length`는 배열의 길이를 알려주는 프로퍼티이다.

`length`값을 수동으로 감소시키면 배열이 잘린다 !

```jsx
let arr = [1, 2, 3, 4, 5];

arr.length = 2; // 요소 2개만 남기고 잘라봅시다.
alert(arr); // [1, 2]

arr.length = 5; // 본래 길이로 되돌려 봅시다.
alert(arr[3]); // undefined: 삭제된 기존 요소들이 복구되지 않습니다.
```

### 5. 다양한 배열 메서드

- `splice(start, deleteCount, ...items)`  
   기존의 배열의 요소를 제거하고, 그 위치에 새로운 요소를 추가한다. → 원본 배열이 변경된다.

  배열 중간에 새로운 요소를 추가할 때도 사용되며, 가장 일반적인 사용은 **배열에서 요소를 삭제할 때 사용**한다.

  ```jsx
  const items1 = [1, 2, 3, 4];

  // items[1]부터 2개의 요소를 제거하고 제거된 요소를 배열로 반환
  const res1 = items1.splice(1, 2);

  // splice를 쓰면 원본 배열이 변경
  console.log(items1); // [ 1, 4 ]

  // 제거한 요소가 배열로 반환된다.
  console.log(res1); // [ 2, 3 ]
  ```

- `slice(start, end)`  
   start 인덱스부터 end 인덱스까지 요소를 복사한다. → 원본 배열 변경 ❌

  ```jsx
  const animals = ["ant", "bison", "camel", "duck", "elephant"];

  // end가 없으니 index 2번부터 끝까지 복사
  console.log(animals.slice(2)); // ["camel", "duck", "elephant"]

  // index2 부터 index4 직전인 index3 까지 복사
  console.log(animals.slice(2, 4)); // ["camel", "duck"]
  ```

- `push()`  
   요소를 배열 맨 끝에 삽입한다. → `스택 stack` 에 사용

  ```jsx
  let fruits = ["사과", "오렌지"];
  fruits.push("배");
  alert(fruits); // 사과,오렌지,배
  ```

- `pop()`  
   배열의 맨 끝 요소를 추출한다. → `스택 stack` 에 사용

  ```jsx
  let fruits = ["사과", "오렌지", "배"];
  alert(fruits.pop()); // 배열에서 "배"를 제거하고 제거된 요소를 얼럿창에 띄웁니다.
  alert(fruits); // 사과,오렌지
  ```

- `shift()`  
   배열의 가장 앞 요소를 꺼내 리턴한다. → `큐 queue`에 사용
  ```jsx
  let fruits = ["사과", "오렌지", "배"];
  alert(fruits.shift()); // 사과
  alert(fruits); // 오렌지,배
  ```
- `unshift()`  
   배열 가장 앞에 요소를 삽입한다.

  ```jsx
  let fruits = ["오렌지", "배"];
  fruits.unshift("사과");
  alert(fruits); // 사과,오렌지,배
  ```

- `concat()`  
   두 개 이상의 배열을 병합한다.

  ```jsx
  const array1 = ["a", "b", "c"];
  const array2 = ["d", "e", "f"];
  const array3 = array1.concat(array2);

  console.log(array1); // ['a', 'b', 'c']
  console.log(array2); // ['d', 'e', 'f']
  console.log(array3); // ["a", "b", "c", "d", "e", "f"]
  ```

- `forEach()`  
   각 배열 요소에 대해 제공된 함수를 한 번씩 실행한다.

  ```jsx
  const array1 = ["a", "b", "c"];

  array1.forEach((element) => console.log(element));

  // Expected output: "a"
  // Expected output: "b"
  // Expected output: "c"
  ```

- `includes(serachElement, fromIndex)`  
   배열 요소에 특정 값이 포함되어 있는지를 판단하여 `true/false`를 리턴한다.

  ```jsx
  const pets = ["cat", "dog", "bat"];

  console.log(pets.includes("cat"));
  // Expected output: true
  console.log(pets.includes("at"));
  // Expected output: false

  console.log([1, 2, 3].includes(3, 2)); // true
  console.log([1, 2, 3].includes(3, 3)); // false
  ```

- `find()`  
   제공된 배열에서 제공된 테스트 함수를 만족하는 **`첫 번째 요소`**를 리턴한다.

  ```jsx
  const array1 = [5, 12, 8, 130, 44];

  // 10보다 큰 요소를 발견하면 그 요소를 반환하면서 반복문이 끝남
  const found = array1.find((element) => element > 10);
  console.log(found); // 12
  ```

- `filter()`  
   주어진 배열에서 제공된 함수에 의해 구현된 **테스트를 통과한 요소들만 필터링하여 새로운 배열을 리턴**한다.

  ```jsx
  const words = ["spray", "elite", "exuberant", "destruction", "present"];

  const result = words.filter((word) => word.length > 6);
  console.log(result);
  // Expected output: Array ["exuberant", "destruction", "present"]
  ```

- `map(callbackFunc)`  
   호출한 배열의 모든 요소에 주어진 함수를 호출한 결과를 채운 새로운 배열 생성

  ```jsx
  const array1 = [1, 4, 9, 16];

  // Pass a function to map
  const map1 = array1.map((x) => x * 2);

  console.log(map1);
  // Expected output: Array [2, 8, 18, 32]
  ```

- `reduce(callback, initialValue)`  
   배열의 각 요소에 대해 주어진 reduce 함수를 실행하고, 하나의 결과값을 반환한다.
  알고리즘 문제를 풀 때 가장 유용하게 자주 사용한 메서드로, 자세한 정리는 [[TIL | JS] reduce() 함수 정리](<[https://velog.io/@gaeunchoi/TIL-JS-reduce에-대해서-알아보자](https://velog.io/@gaeunchoi/TIL-JS-reduce%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)>)를 참고!

  ```jsx
  const array1 = [1, 2, 3, 4];

  // 0 + 1 + 2 + 3 + 4
  const initialValue = 0;
  const sumWithInitial = array1.reduce(
    (accumulator, currentValue) => accumulator + currentValue,
    initialValue
  );

  console.log(sumWithInitial);
  // Expected output: 10
  ```

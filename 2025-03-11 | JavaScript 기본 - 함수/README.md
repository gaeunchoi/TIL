# JavaScript 정리 - 2. 함수

## 1️⃣ 함수

JS에서 함수는 일급 객체로, `변수에 담을 수 있다`, `파라미터로 전달할 수 있다`, `반환값으로 사용할 수 있다.` 등의 특징이 있다.

## 2️⃣ 지역변수 & 외부변수(전역변수)

`지역 변수`: 함수 내에서 선언한 변수로 `함수 안에서만 접근 가능`하다.

```jsx
function showMessage() {
  let msg = "show message"; // msg는 지역변수
  console.log(msg);
}
showMessage(); // show message
console.log(msg); // ReferenceError: msg is not defined
```

<br>

`외부 변수`: 함수 외부에서 선언한 변수로, 함수 안에서도 `접근 및 수정`이 가능하다.

```jsx
let userName = "Lisa";

// 외부 변수 접근하기
function showMessage() {
  let msg = "Hello, " + userName;
  console.log(msg);
}

// 외부 변수 수정하기
function showMessage() {
  userName = "Bob";

  let msg = "Hello, " + userName;
  console.log(msg);
}
```

## 3️⃣ 매개변수

함수에 필요한 데이터를 전달하는 요소로 `매개변수` 또는 `인자`라고 부른다.  
아래 예시에서 함수명 옆 괄호에 들어있는 **text, writer**가 매개변수가 된다.

```jsx
function showMessage(text, writer) {
  console.log(`${writer} : ${text}`);
}
```

## 4️⃣ 함수 이름 작성 방법

함수가 어떤 동작을 하는지 축약해서 설명해주는 동사를 접두어로 붙여 함수 이름을 만드는게 관습이다.

다음 접두어를 이용해서 작성해주면 좋다 !

- `show…` 로 시작하는 함수 : 무언가를 보여주는 함수
- `get…` 로 시작하는 함수 : 값을 반환함
- `calc…` 로 시작하는 함수 : 무언가를 계산함
- `create…` 로 시작하는 함수 : 무언가를 생성함
- `check…` 로 시작하는 함수 : 무언가를 확인하고 불린값을 반환함

## 5️⃣ 함수 표현식 vs 함수 선언식 vs 화살표 함수

### 1. 함수 선언문

```jsx
function 함수명() {
	함수 로직 와랄라
}
```

### 2. 함수 표현식

함수를 생성하고 변수에 값을 할당하는 것 처럼 함수를 할당할 수 있다.

```jsx
let sayHi = function () {
  console.log("hi");
};
console.log(sayHi); //ƒ () {console.log('hi')} -> 함수 자체가 보임
sayHi(); // hi -> 함수가 실행됨(자바스크립트는 괄호가 있어야 함수가 실행됨)

// 함수 복사도 가능
function sayHi() {
  // (1) 함수 생성
  console.log("hi");
}

let func = sayHi; // (2) 함수 복사

func(); // hi     // (3) 복사한 함수를 실행
sayHi(); // hi    // (4) 본래 함수도 정상적으로 실행됩니다.

// 함수 표현식 안에서도 종류가 있습니다.
// 기명 함수 표현식 vs 익명 함수 표현식

// 기명 함수 표현식(named function expression)
var foo = function multiply(a, b) {
  return a * b;
};

// 익명 함수 표현식(anonymous function expression)
var bar = function (a, b) {
  return a * b;
};

console.log(foo(10, 5)); // 50
console.log(multiply(10, 5)); // Uncaught ReferenceError: multiply is not defined
```

### 3. 콜백함수

`콜백함수 callback`란 매개변수로 전달하는 함수를 의미한다.

함수에 콜백함수를 전달해서 함수를 사용할 때 내가 원하는 시점에 콜백함수를 사용한다.

```jsx
// 함수에 질문을 하면, 사용자 답변에 따라 yes / no 를 출력하는 함수입니다.
function ask(question, yes, no) {
  if (confirm(question)) yes();
  else no();
}

function showOk() {
  alert("동의하셨습니다.");
}

function showCancel() {
  alert("취소 버튼을 누르셨습니다.");
}

// 사용법: 함수 showOk와 showCancel가 ask 함수의 인수로 전달됨
// 즉, 여기서 showOk, showCancel 가 콜백 함수 라고 불리는 친구들
ask("동의하십니까?", showOk, showCancel);
```

### 4. 화살표 함수

함수의 표현을 한 줄로 간결하게 작성할 때 사용한다. `function` 키워드를 지우고 화살표를 이용한다.

```jsx
// 함수 선언식
function sum(a, b) {
  return a + b;
}

// 화살표 함수 ex 1
const sum = (a, b) => {
  return a + b;
};

// 만약 return 문 1줄만 필요할 경우 중괄호 생략 가능
const sum = (a, b) => a + b;
```

### 5. 함수 선언문과 함수 표현식의 차이

- `함수 선언문`: 함수 선언문이 정의되기 전에도 호출할 수 있다.
- `함수 표현식`: 실제 실행 흐름이 해당 함수에 도달했을 때 함수가 생성된다.
  즉, 코드 실행 흐름이 함수에 도달했을 때부터 해당 함수를 사용할 수 있다.

  ```jsx
  // 함수 선언문
  // 아래 코드 실행해보면 함수 정의한 것보다 함수 호출한 것이 위에 있어도 정상 동작됨
  sayHelloToName("lisa"); // Hello, lisa

  function sayHelloToName(name) {
    console.log(`Hello, ${name}`);
  }

  // 함수 표현식
  // 반면, 아래 코드는 함수 정의하기 전에 함수를 호출했을 때 아직 정의되지 않았기 때문에 에러남
  sayHelloToName2("lisa"); // ReferenceError: sayHelloToName2 is not defined

  let sayHelloToName2 = function (name) {
    console.log(`Hello, ${name}`);
  };
  ```

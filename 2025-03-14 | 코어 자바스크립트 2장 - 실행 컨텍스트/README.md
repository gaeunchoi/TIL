## 1️⃣ 실행 컨텍스트 Execution Context

### 1. 실행 컨텍스트란

`실행 컨텍스트 execution context` 란 실행할 코드에 제공할 환경 정보들을 모아놓은 객체로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념이다.

여기서, 코드에 제공할 `환경 정보`는 자바스크립트 엔진이 호출되는 함수와 관련된 정보(=환경정보)를 수집하여 실행 컨텍스트 객체에 전달/저장할 때 생성된다.

`환경 정보` 는 아래와 같은 정보를 포함한다.

- `VariableEnvironment`: 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보. 선언 시점의 LexicalEnvironment의 스냅샷(변경사항 추적x)
- `LexicalEnvironment`: 초기화 과정에서 VariableEnvironment와 동일하지만, 변경사항 실시간 추적 o
- `ThisBinding`: this 식별자가 바라봐야 할 대상 객체

### 2. 콜스택

```jsx
// ---------------------------- (1)
var a = 1;
function outer() {
  function inner() {
    console.log(a); //undefined
    var a = 3;
  }
  inner(); // ---------------- (2)
  console.log(a); //1
}
outer(); // ------------------ (3)
console.log(a); //1
```

1. 자바스크립트 파일이 실행되면 `(1)` , **전역 컨텍스트**가 콜스택이 담긴다.  
   실행 컨텍스트는 어떤 함수가 호출되면 열리지만, 전역 컨텍스트는 별도의 실행 명령이 없어도 브라우저에서 자동으로 실행된다.

2. 콜 스택에 전역 컨텍스트 외에 다른 덩어리가 없으니 전역 컨텍스트와 관련된 코드들을 순차로 진행하다가, `(3)`에서 **outer** 함수를 호출하면 자바스크립트 엔진이 **outer**에 대한 환경 정보를 수집 후 콜스택에 담는다.  
   이 때, 콜스택 최상단에 **outer** 실행 컨텍스트가 놓였으므로 **전역 컨텍스트**는 다시 상단에 오기 전까지는 잠시 중단된다.

3. **outer** 실행 컨텍스트가 열리면서 outer 함수 내부의 코드가 순차적으로 실행되고, `(2)` 지점에서 **inner** 함수가 호출되며 **inner** 실행 컨텍스트가 콜스택에 담기게 된다. 결론적으로 콜스택의 최상단은 inner가 된다.
4. **inner** 함수 내부의 코드가 순차적으로 실행된 후에 콜스택에서 **inner** 함수는 pop된다.  
   다시 **outer** 함수가 콜 스택의 최상단을 차지하고, 마찬가지로 **outer**도 모든 코드를 실행하면 콜스택에서 빠져나온다.  
   마지막으로 **전역 컨텍스트**만 남은 콜스택은 마저 전역 컨텍스트를 실행시키고 pop시킨다.

   ![실행 컨텍스트와 콜스택](https://velog.velcdn.com/images/gaeunchoi/post/88b75d7b-8cba-4fbc-9d15-45ea7638f87d/image.png)

<br>

## 2️⃣ VariableEnvironment

`VariableEnvironment` 에 담기는 내용은 `LexicalEnvironment`와 같지만, 최초 실행 시의 스냅샷을 유지한다는 점(변경사항 실시간 추적 유무)이 다르다.

`VariableEnvironment`, `LexicalEnvironment` 의 내부는 `environmentRecord` 와 `outerEnvironmentReference` 로 구성된다.

<br>

## 3️⃣ LexicalEnvironment

`Lexical Environment`: **어휘적 환경** 혹은 **정적 환경** 이라는 말로 주로 번역되나, 수시로 변하는 환경 정보라는 뜻에서 보자면 **사전적인 환경**으로 표현된다.

### 1. environmentRecord와 호이스팅(hoisting)

`environmentRecord`는 **현재 컨텍스트 내부**의 식별자 정보들이 저장된다.  
컨텍스트를 구성하는 함수의 매개변수 식별자, 컨텍스트 내부 함수, var, let 등으로 선언된 변수 식별자 등이 `environmentRecord`의 수집 대상에 포함된다.

여기서, 컨텍스트 내부의 식별자를 수집하는 것과 실행되는 것은 별개의 작업이다.  
다시말해, 코드가 실행되기 전임에도 불구하고 자바스크립트 엔진은 이미 해당 환경에 속한 코드의 변수명들을 모두 파악하게 되는데 이를 `호이스팅 hoisting`이라고 부른다.

<br>

- 호이스팅 Hoisting  
  `호이스팅` 은 컨텍스트 내부의 변수를 끌어올린다는 의미로 알려져있다.

  이는 일종의 가상 개념으로, 자바스크립트 엔진이 식별자를 미리 파악한 것을 일종의 **끌어올림**으로 형상화하여 표현한 개념이다.(실제로 식별자를 끌어올리는 것 ❌)
  <br>

- 함수 선언문과 함수 표현식  
  `함수 선언문 function declaration`: **function** 정의부만 존재하고 별도의 할당 명령이 없는 것을 의미한다. 반드시 함수명이 존재해야한다.

  `함수 표현식 function expression`: **function**을 별도의 변수에 할당하는 것을 의미한다. 함수명이 반드시 정의될 필요 없이, 할당된 식별자를 곧 함수명으로 부른다.

```jsx
function a() {...}          // 함수 선언문, 함수명 a가 곧 변수명이다.
var b = function () {...}   // (익명) 함수 표현식, 변수명 b가 곧 함수명이다.
var c = function d() {...}  // 기명 함수 표현식, 변수명은 c, 함수명은 d이다.

a();  // 실행 ok
c();  // 실행 ok
d();  // 에러 발생
```

```jsx
// 호이스팅과 함께 함수 선언문 & 표현식을 보자
// before hoisting
console.log(sum(1, 2));
console.log(multiply(3, 4));

function sum(a, b) {
  // 함수 선언문
  return a + b;
}

var multiply = function (a, b) {
  // 함수 표현식
  return a * b;
};

// after hoisting
var sum = function sum(a, b) {
  // (1)
  return a + b;
};

var multiply; // (2)

console.log(sum(1, 2));
console.log(multiply(3, 4));

multiply = function (a, b) {
  return a * b;
};
```

> `호이스팅`을 통해 **sum**과 **multiply**가 console.log()보다 위로 끌어올려졌다.  
> 이 때 **sum**은 `함수 선언문`이기 때문에 하나의 값으로써 전체가 끌어올려진다. <br> **sum 변수**가 메모리 공간의 어떠한 곳을 차지하고, **sum 함수** 역시 메모리 공간의 다른 곳에 저장된다. <br> **sum 변수**가 저장된 메모리 공간은 **sum 함수**의 주소를참조하고, 해당 함수가 해당 변수에 할당된 것을 알 수 있다. <br>
> 반대로, **multiply**는 `함수 표현식`이기 때문에 변수 multiply만 호이스팅되고, 해당 메모리 공간이 참조할 주소는 비어있다. <br>
> 이 상태로 console.log(multiply(3, 4))를 찍어보면 에러메세지가 출력된다. 왜냐하면, **multiply**는 그저 식별자이기 때문이다. <br> `environmentRecord`의 관점에서 보면, 해당 컨텍스트의 `environmentRecord`는 sum 식별자와 함수정의부, 식별자 뿐인 multiply만을 저장한 것이다.

<br>

- 상대적으로 `함수 표현식`이 더 안전하다

  **A**가 먼저 개발을 하면서 `함수 F`를 선언하고, 이 후 **B**가 개발에 참여하면서 `함수 F`를 새로 선언하게 되었다고 가정하자.  
  **B**가 `함수 F`를 새로 선언하면 전역 컨텍스트가 활성화 될 때 전역 공간에 선언된 함수들이 모두 가장 위로 끌어올려진다.  
  동일한 변수명에 서로 다른 값을 할당할 경우 **나중에 할당한 값이 먼저 할당한 값을 덮어씌우기 때문(오버라이드)** 에 실제로 호출되는 함수는 **B**가 선언한 `함수 F`뿐이다.
  이러한 상황이 에러를 발생하진 않겠지만, **A**가 사용하던 `함수 F`로 원하던 결과값을 얻지 못할것이다.

  반면, `함수 표현식`은 **A**가 원했던 부분에서는 **A**의 의도대로, **B**가 작성한 부분에서는 **B**의 의도대로 잘 동작한다.

### 2. 스코프, 스코프 체인, outerEnvironmentReference

- `스코프 scope`: 식별자에 대한 유효범위(접근할 수 있는 영역이라고 생각하면 된다.)
  자바스크립트는 함수에 의해 스코프가 발생한다. ES6 부터는 블록에 의해서도 스코프 경계를 발생시킬 수 있다.

```jsx
    // a 식별자는 전역공간 뿐만 아니라 화살표함수인 bScope() 내부까지 접근 가능
    const a = "a";

    const bScope = () => {
    		const b = "b";         // b 식별자는 bScope() 내부에서 선언되었기 때문에 외부에서 접근 ❌
    		...
    }
```

- 스코프 체인
  `LexicalEnvironment` 는 `outerEnvironmentReference`와 `environmentRecord`로 이루어져 있다. <br>

  그 중, `outerEnvironmentReference`는 호출된 함수가 **선언될 당시**의 `LexicalEnvironment`를 참조한다.
  → `outerEnvironmentReference`는 연결리스트(링크드리스트) 형태를 띈다.
  → 따라서, 순서대로 접근해야 하고, 이런 특징으로 인해 여러 스코르에서 동일한 식별자를 선언한 경우, 무조건 **스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근이 가능**하다.

```jsx
var a = 1;
var outer = function () {
  var inner = function () {
    console.log(a); // undefined로 초기화 된 지역변수 a에 접근
    var a = 3;
  };
  inner();
  console.log(a); // 전역 변수 a 접근
};
outer();
console.log(a); // 전역 변수 a 접근
```

- 변수 은닉화  
   위의 코드에서 `inner` 함수 내부에서 a 변수를 선언했기 때문에 inner 함수에서는 전역 변수 a에 접근이 불가능하다. 이를 변수 은닉화라고 한다.
  > 💡 은닉화란 ?
  > 객체의 데이터와 메서드를 외부에서 직접 접근하지 못하도록 제한하는 개념이다.
- 지역변수와 전역변수  
  `지역변수`: 전역 공간에 선언한 변수

  `전역변수`: 함수 내부에서 선언한 변수

  → 코드의 안정성을 위해 `전역변수` 사용을 최소화하는 것이 좋다.

## 4️⃣ this

실행 컨텍스트의 `thisBinding`에는 `this`로 지정된 객체가 저장된다.

실행 컨텍스트 활성화 당시에 `this`가 지정되지 않은 경우네는 전역 객체가 저장된다.

그 밖에 함수를 호출하는 방법에는 `this`에 저장되는 대상이 다르다. → 코어 자바스크립트 3장에서 계속 !

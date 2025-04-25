`this`란 대부분 객체 지향 언어에서 클래스로 생성한 인스턴스 객체를 의미한다.

그러나 JS에서의 `this`는 어디에서든 사용이 가능하다. 하지만 상황에 따라 `this`가 바라보는 대상이 달라진다.

→ 함수와 객체(메서드)의 구분이 느슨한 자바스크립트에서 `this`는 실질적으로 이 둘을 구분하는 유일한 기능이다.

## 1️⃣ 상황에 따라 달라지는 this

- `this`는 기본적으로 실행 컨텍스트가 생성될 때 함께 결정된다.
- 실행 컨텍스트는 함수를 호출할 때 생성된다 === **`this`는 함수를 호출할 때 결정**된다.

### 1. 전역 공간에서의 this

- 전역 공간에서의 `this` === **전역객체**
- 브라우저 환경에서 전역객체 === **window**
- Node.js 환경에서 전역객체 === **global**

전역변수를 선언하면 자바스크립트 엔진은 이를 전역객체의 프로퍼티로 할당한다.

```jsx
var a = 1;
console.log(a); // 1
console.log(window.a); // 1 **→** 브라우저 환경에서
console.log(global.a); // 1 **→** Node.js 환경에서
console.log(this.a); // 1 **→** 전역 스코프에서, 브라우저 환경에서는 window.a와 같다
```

- 자바스크립트 엔진은 var 키워드를 이용해 변수를 선언하면 실행 컨텍스트의 L.E(LexicalEnvironment), 즉 전역 객체에서 해당 프로퍼티 a를 발견해 그 값을 반환하기 때문에 1이 출력된다.
- const와 let를 이용하여 선언하면 → window.a와 this.a에 undefined가 출력된다.  
   왜냐하면, 이는 전역 객체로 저장되지 않고 보이지않는 개념적인 블록 내에 존재하기 때문이다.

전역변수 선언과 전역객체의 프로퍼티 할당 사이에 전혀 다른 경우 → **삭제** 명령

```jsx
var a = 1;
delete window.a; // false

var b = 2;
delete b; // false

window.c = 3;
delete window.c; // true

window.d = 4;
delete d; // true
```

- 처음부터 전역객체의 프로퍼티로 할당한 경우 → 삭제 ⭕️
- 전역변수로 선언한 경우 → 삭제 ❌
- 사용자가 의도치않게 삭제하는 것을 방지하는 차원에서 마련한 나름의 방어 전략임 ..

<br>

### 2. 함수 vs 메서드

어떤 함수를 실행하는 방법에 여러가지 방법이 있는데, 가장 일반적인 두가지 방법은 **함수로서 호출**하는 경우와 **메서드로서 호출**하는 경우이다.

이 둘을 구분하는 유일한 차이는 **독립성**에 있다.

- 함수 → 그 자체로 독립적인 기능을 수행
- 메서드 → 자신을 호출한 대상 객체에 관한 동작을 수행

함수로서, 메소드로서 호출하는 방법은 다음과 같이 구분할 수 있다.

- 어떤 함수를 호출할 때 그 함수 이름(프로퍼티명) 앞에 객체가 명시된 경우 → 메서드로 호출
- 그렇지 않은 모든 경우 → 함수로 호출

```jsx
var func = function (x) {
  console.log(this, x);
};
func(1);

var obj = {
  method: func,
};
obj.method(2);
obj["method"](2);
```

### 2-1. 메서드로서 호출할 때 그 메서드 내부에서의 this

`this`에는 호출한 주체에 대한 정보가 담긴다.

어떤 함수를 메서드로서 호출하는 경우 호출 주체는 바로 함수명(프로퍼티명) 앞의 객체이다.

```jsx
var obj = {
	methodA: function (){
		console.log(this);
	}
	inner: {
		methodB: function() {
			console.log(this);
		}
	}
}

obj.methodA(1);        // { methodA: f, inner: {...} } (===obj)
obj['methodA']();      // { methodA: f, inner: {...} } (===obj)

obj.inner.methodB();         // { methodB: f } (===obj.inner)
obj.inner['methodB']();      // { methodB: f } (===obj.inner)
obj['inner'].methodB();      // { methodB: f } (===obj.inner)
obj['inner']['methodB']();   // { methodB: f } (===obj.inner)
```

### 2-2. 함수로서 호출할 때 함수 내부에서의 this

1. 함수 내부에서의 this

- 어떤 함수를 함수로서 호출할 경우에는 `this`가 지정되지 않는다. 이는 호출 주체(객체)를 명시하지 않고 개발자가 코드에 직접 관여해 실행한 것이기 때문에 호출 주체의 정보를 알 수 없기 때문이다.
- 따라서, 함수에서 `this`는 전역객체를 가리킨다.

2. 메서드의 내부함수에서의 this

- `this 바인딩`에 관해서는 함수를 실행하는 당시의 주변 환경(메서드 내부인지, 함수 내부인지 등)은 중요하지 않고, 오직 해당 함수를 호출하는 구문 앞에 **점 또는 대괄호 표기**가 있는지 없는지가 관건이다.

```jsx
var obj1 = {
  outer: function () {
    console.log(this); // (1) { outer: f } (===obj1)

    var innerFunc = function () {
      console.log(this); // (2) Window { parent: ... } (===Window)
      // (3) { innerMethod: f } (===obj2)
    };
    innerFunc(); // (2) 실행

    var obj2 = {
      innerMethod: innerFunc,
    };
    obj2.innerMethod(); // (3) 실행
  },
};

obj1.outer(); // (1) 실행
```

3. this를 바인딩하지 않는 함수  
   ES6에서 함수 내부에서 `this`가 전역 객체를 바라보는 문제를 보완하기 위해 this를 바인딩하지 않는 **`화살표 함수 arrow function`**을 새로 도입했다. <br>
   `화살표 함수`는 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 빠지게 되어 상위 스코프의 this를 그대로 활용할 수 있다.

```jsx
var obj = {
  outer: function () {
    console.log(this); // { outer: f }

    var innerFunc = () => {
      console.log(this); // { outer: f }
    };

    innerFunc();
  },
};

obj.outer();
```

### 3. 콜백 함수 호출 시 그 함수 내부에서의 this

코어 자바스크립트 4장 콜백함수 챕터에서 자세히 다룰 예정 ! (링크 추가 예정)

- `콜백 함수` 란 함수 A의 제어권을 다른 함수(또는 메서드) B에게 넘겨줄 때 함수 A를 콜백함수라고 한다.
- 콜백 함수에서의 `this`는 상황에 따라 변한다.
- 제어권을 가지는 함수(메서드)가 콜백함수에서 `this`를 무엇으로 할 지 결정하고, 특별히 정의하지 않았다면 전역객체를 바라본다.

### 4. 생성자 함수 내부에서의 this

코어 자바스크립트 7장 클래스 챕터에서 자세히 다룰 예정 ! (링크 추가 예정)

- `생성자 함수`: 어떤 공통된 성질을 지니는 객체들을 생성하는데 사용하는 함수
- 자바스크립트는 `new` 키워드와 함께 함수를 호출하면 해당 함수가 생성자로서 동작한다.
- 어떤 함수가 생성자로 호출로서 호출된 경우 내부에서의 `this`는 곧 새로 만들 구체적인 인스턴스 자신이다.
-

## 2️⃣ 명시적으로 this를 바인딩하는 방법

### 1. call 메서드

```jsx
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

- `call 메서드`: 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령이다.
- call 메서드의 첫 번째 인자를 `this`로 바인딩하고, 이후의 인자들을 호출할 함수의 매개변수로 한다.
- 즉, 함수를 그냥 실행하면 `this`는 전역객체를 참조하지만, call 메서드를 이용하면 임의의 객체를 `this`로 지정 가능하다.

```jsx
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

func(1, 2, 3); // Window{ ... } 1 2 3
func.call({ x: 1 }, 4, 5, 6); // { x:1 } 4 5 6
```

### 2. apply 메서드

```jsx
Function.prototype.apply(thisArg[, argsArray])
```

- `apply 메서드`: call 메서드와 기능적으로 완전히 동일하다. 다만, apply 메서드는 두 번째 인자를 배열로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다.

```jsx
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

func(1, 2, 3); // Window{ ... } 1 2 3
func.apply({ x: 1 }, [4, 5, 6]); // { x:1 } 4 5 6
```

### 3. bind 메서드

```jsx
Function.prototype.bind(thisArg[, arg1[, arg2[, ...]]])
```

- ES5에서 추가된 기능으로, call 메서드와 비슷하지만 즉시 호출하지는 않고 넘겨 받은 this 및 인수들을 바탕으로 새로운 함수를 반환한다.
- 즉, `bind 메서드` 는 함수에 this를 미리 적용하는 것과 부분 적용 함수를 구현하는 두 가지 목적을 지님

```jsx
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
};
func(1, 2, 3, 4); // Window{...} 1 2 3 4

var bindFunc1 = func.bind({ x: 1 });
bindFunc1(5, 6, 7, 8); // { x: 1 } 5 6 7 8

var bindFunc2 = func.bind({ x: 1 }, 4, 5);
bindFunc2(6, 7); // { x: 1} 4 5 6 7
bindFunc2(8, 9); // { x: 1} 4 5 8 9
```

### 4. 화살표 함수의 예외사항

- ES6에 새롭게 도입된 화살표 함수는 실행 컨텍스트 생성 시 this를 바인딩 하는 과정이 제외된다.
- 즉, 이 함수 내부에는 this가 아예 없으며, 접근하고자 하면 스코프체인 상 가장 가까운 this에 접근한다.

```jsx
var obj = {
  outer: function () {
    console.log(this); // { outer: [Function: outer] }

    var innerFunc = () => {
      console.log(this); // { outer: [Function: outer] }
    };
    innerFunc();
  },
};
obj.outer();
```

### 5. 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)

- 콜백 함수를 인자로 받는 메서드 중 일부는 추가로 this를 지정할 객체(thisArg)를 인자로 지정할 수 있는 경우가 있다.
- 배열 메서드에 이런 경우가 많이 있으며, 대표적인 예로 forEach가 있다.
- forEach, map, filter, some, every, find, findIndex, flatMap, from, Set, Map 등이 있다.

```jsx
var report = {
  sum: 0,
  count: 0,
  add: function () {
    var args = Array.prototype.slice.call(arguments);
    args.forEach(function (entry) {
      this.sum += entry;
      ++this.count;
    }, this);
  },
  average: function () {
    return this.sum / this.count;
  },
};

report.add(60, 85, 95);
console.log(report.sum, report.count, report.average()); // 240 3 80
```

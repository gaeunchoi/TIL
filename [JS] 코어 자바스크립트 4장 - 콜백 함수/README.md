## 1️⃣ 콜백함수란?

- `콜백 함수 callback function`: 다른 코드의 인자로 넘겨주는 함수를 의미한다.
- 어떤 함수 A를 호출하면서 **특정 조건일 경우 함수 B를 실행해서 나에게 알려달라는 요청**을 함께 보내는 것으로써, 함수 A의 입장에서는 해당 조건이 갖춰져있는지 여부를 판단하고, 함수 B를 직접 호출할 수 있는 `제어권`을 가진다.
  <br>

---

<br>

## 2️⃣ 제어권

### 2-1-1. 콜백함수 예제 → setInterval 메서드

```jsx
var intervalId = scope.setInterval(func, delay[, param1, param2, ...]);
```

- `setInterval` 메서드의 매개변수로는 `func`, `delay` 값을 반드시 전달해야한다.
- func에 넘겨준 함수는 매 delay(ms)마다 실행되고, 그 어떠한 값도 리턴하지 않는다.

> 💡 setInterval을 실행하면 반복적으로 실행되는 내용 자체를 특정할 수 있는 고유한 ID값이 반환된다.

### 2-1-2. 호출 시점

```jsx
var cnt = 0;
var cbFunc = setInterval(function() {
	console.log(cnt);
	if(++cnt > 4) clearInterval(timer);
};

var timer = setInterval(cbFunc, 300);
```

- timer 변수에는 setInterval의 ID값이 담긴다.
- setInterval에 전달한 첫 번째 인자인 **cbFunc 함수(= 콜백함수)**는 0.3초마다 자동으로 실행된다.
- 첫 번째 인자로서 cbFunc 함수를 넘겨주자 제어권을 넘겨받은 setInterval이 스스로의 판단에 따라 0.3초마다 익명 함수를 실행했다.

> 💡 **콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수 호출 시점에 대한 제어권을 가진다.**

| code                      | 호출 주체   | 제어권      |
| ------------------------- | ----------- | ----------- |
| cbFunc();                 | User        | User        |
| setInterval(cbFunc, 300); | setInterval | setInterval |

<br>

### 2-2-1. 콜백함수 예제 → map 메서드

```jsx
Array.prototype.map(callback[, thisArg[)
callback: function(currentValue, index, array)
```

- `map` 메서드는 메서드 대상이 되는 배열의 모든 요소들을 처음부터 끝까지 하나씩 꺼내 콜백 함수를 반복 호출하고, 콜백 함수의 실행 결과를 모아 새로운 배열을 만든다.
- 콜백함수의 첫번째 인자 : 배열의 요소 중 현재 값
- 콜백함수의 두번째 인자: 현재값의 인덱스
- 콜백함수의 세번째 인자: 메서드 대상 배열

### 2-2-2. 인자

```jsx
var newArr = [10, 20, 30].map(function (cur, idx, arr) {
  console.log(cur, idx, arr);
  return cur + 5;
});
// 10 0 [10, 20, 30]
// 20 1 [10, 20, 30]
// 30 1 [10, 20, 30]

console.log(newArr); // (3) [15, 25, 35]
```

- 첫 번째(인덱스 0)에 대한 콜백함수는 cur에 10, idx에 0, arr에 [10, 20, 30]이 담긴채 실행
- 두 번째(인덱스 1)에 대한 콜백함수는 cur에 20, idx에 1, arr에 [10, 20, 30]이 담긴채 실행
- 세 번째(인덱스 2)에 대한 콜백함수는 cur에 30, idx에 2, arr에 [10, 20, 30]이 담긴채 실행

❓ 이 때 넘기는 인자의 순서를 바꾸면 어떻게 될까

```jsx
var newArr = [10, 20, 30].map(function (idx, cur, arr) {
  console.log(idx, cur, arr);
  return cur + 5;
});
// 10 1 [10, 20, 30]
// 20 1 [10, 20, 30]
// 30 1 [10, 20, 30]

console.log(newArr); // (3) [5, 6, 7]
```

- [15, 25, 35]가 아닌 [5, 6, 7]이라는 전혀 다른 값이 반환된다.
- cur로 명명한 인자의 위치가 두 번째이므로 컴퓨터가 여기에 인덱스 값을 부여했기 때문이다.
- 즉, `map 메서드`를 호출해서 원하는 배열을 얻으려면 **map 메서드에 정의된 규칙에 따라 작성**해야한다.

> 💡 **콜백함수의 제어권을 넘겨받은 코드는 콜백함수를 호출할 때 인자에 어떤 값들을 순서로 넘길것인지에 대한 제어권을 가진다.**

<br>

### 2-3. this

```jsx
setTimeout(function () { console.log(this); }, 300);             //  -------- (1)

[1, 2, 3, 4, 5].forEach(function(x) {
	console.log(this);                                             //  -------- (2)
});

document.body.innerHTML += "<button id="a">클릭</button>";
document.body.querySelector("#a").addEventListener("click", function(e) {
	console.log(this, e);                                          //  -------- (3)
});
```

- (1) `setTimeout`은 내부에서 콜백 함수를 호출할 때 call 메서드의 첫 번째 인자에 전역 객체를 넘기기 때문에 콜백 함수 내부에서의 this는 **전역객체**를 가리킨다.
- (2) `forEach`는 별도의 인자를 this로 받을 경우 그 인자에 해당하지만, 별도의 인자를 넘겨주지 않았기에 **전역객체**를 가리킨다.
- (3) `addEventListener`메서드의 this를 그대로 넘기도록 정의되어 있기 때문에 this는 이벤트리스너를 **호출한 주체인 HTML 엘리먼트**를 가리킨다.

<br>

---

<br>

## 3️⃣ 콜백 함수는 함수다

콜백함수로 **어떤 객체의 메서드를 전달하더라도 그 메서드는 함수로서 호출**된다.

```jsx
var obj = {
  vals: [1, 2, 3],
  logValues: function (v, i) {
    console.log(this, v, i);
  },
};

obj.logValues(1, 2); //  -------- (1)
[4, 5, 6].forEach(obj.logValues); //  -------- (2)
```

- (1) obj 객체의 logValues는 메서드로서 호출되어 this는 obj를 가리킨다.
- (2) logValues 메서드를 forEach의 콜백함수로 전달했고, obj.logValues는 this가 별도로 저장되지 않았으므로 this는 전역객체를 바라본다.

<br>

---

<br>

## 4️⃣ 콜백 함수 내부의 this에 다른 값 바인딩하기

객체의 메서드를 콜백 함수로 전달하게 되면 해당 객체를 this로 바라볼 수 없게 된다.

그럼에도, 함수 내부에서 this가 객체를 바라보게 하고 싶다면 다음과 같이 할 수 있다.

### 4-1. 콜백 함수 내부의 this에 다른 값을 바인딩 하는 방법 - 클로저

```jsx
var obj1 = {
  name: "obj1",
  func: function () {
    var self = this;
    return function () {
      console.log(self.name);
    };
  },
};
var callback = obj1.func();
setTimeout(callback, 1000);
```

전통적으로 this를 다른 변수에 담아 콜백함수로 활용할 함수에서는 this 대신 그 변수를 사용하고, 클로저를 만드는 방식을 사용했다.

그러나, 실제로 this를 사용하지 않을 뿐더러 번거롭다.

### 4-2. 콜백 함수 내부의 this에 다른 값을 바인딩 하는 방법 - bind

```jsx
var obj1 = {
  name: "obj1",
  func: function () {
    return function () {
      console.log(this.name);
    };
  },
};
setTimeout(obj1.func().bind(obj1), 1000); // ---- obj1

var obj2 = {
  name: "obj2",
};
setTimeout(obj1.func().bind(obj2), 1000); // ---- obj2
```

ES5에서 등장한 `bind` 메서드를 이용해 위 방식의 아쉬움을 보완할 수 있다.

## 5️⃣ 콜백 지옥과 비동기 제어

`콜백 지옥 callback hell`은 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상으로, 자바스크립트에서 흔히 발생하는 문제이다.

주로 별도의 요청(setTimeout), 실행 대기(addEventListner), 보류(XMLHttpRequest) 등과 관련된 코드는 비동기적인 코드이다.

### 5-1. 콜백 지옥

아래 코드는 콜백 지옥의 예시이다.

```jsx
setTimeout(
  function (name) {
    var coffeeList = name;
    console.log(coffeeList);

    setTimeout(
      function (name) {
        coffeeList += ", " + name;
        console.log(coffeeList);

        setTimeout(
          function (name) {
            coffeeList += ", " + name;
            console.log(coffeeList);

            setTimeout(
              function (name) {
                coffeeList += ", " + name;
                console.log(coffeeList);
              },
              500,
              "카페라떼"
            );
          },
          500,
          "카페모카"
        );
      },
      500,
      "아메리카노"
    );
  },
  500,
  "에스프레소"
);

// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 에스프레소, 아메리카노, 카페모카, 카페라떼
```

들여쓰기 수준이 과도하게 깊어졌을뿐더러 값이 전달되는 순서가 아래에서 위로 향하고 있어 어색하게 느껴진다.

가독성 문제와 어색함을 동시에 해결하는 방법 → **익명의 콜백 함수를 모두 기명함수로 전환**

```jsx
var coffeeList = '';

var addEspresso = fuction(name){
    coffeeList = name;
    console.log(coffeeList);
    setTimeout(addAmericano, 500, '아메리카노');
};

var addEspresso = function(name){
    coffeeList = name;
    console.log(coffeeList);
    setTimeout(addAmericano, 500, '아메리카노');
};

var addAmericano = function(name){
    coffeeList += ',' + name;
    console.log(coffeeList);
    setTimeout(addMocha, 500, '카페모카');
};

var addMocha = function(name){
    coffeeList += ',' + name;
    console.log(coffeeList);
    setTimeout(addLatte, 500, '카페라떼');
};

var addLatte = function(name){
    coffeeList = name;
    console.log(coffeeList);
};

setTimeout(addEspresso, 500, '에스프레소');

// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 카페라떼
```

### 5-2. 비동기제어

JS는 비동기적인 작업을 동기적으로, 혹은 동기적인 것처럼 보이게 하기위해 ES6에서는 Promise, Generator가 도입됐고, ES2017에서는 async/await가 도입되었다.

- 비동기 작업의 동기적 표현(1) → Promise(1)

```jsx
new Promise(function (resolve) {
  setTimeout(function () {
    var name = "에스프레소";
    console.log(name);
    resolve(name);
  }, 500);
})
  .then(function (prevName) {
    return new Promise(function (resolve) {
      setTimeout(function () {
        var name = prevName + ", 아메리카노";
        console.log(name);
        resolve(name);
      }, 500);
    });
  })
  .then(function (prevName) {
    return new Promise(function (resolve) {
      setTimeout(function () {
        var name = prevName + ", 카페모카";
        console.log(name);
        resolve(name);
      }, 500);
    });
  })
  .then(function (prevName) {
    return new Promise(function (resolve) {
      setTimeout(function () {
        var name = prevName + ", 카페라떼";
        console.log(name);
        resolve(name);
      }, 500);
    });
  });

// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 에스프레소, 아메리카노, 카페모카, 카페라떼
```

new 연산자와 함께 호출한 Promise의 인자로 넘겨주는 콜백함수는 바로 실행되지만

그 내부의 **resolve** 또는 **reject** 함수를 호출하는 구문이 있을 경우, 둘 중 하나가 실행되기 전까지는 **다음(then)** 또는 **오류 구문(catch)**로 넘어가지 않는다.

> 💡 **비동기 작업이 완료될 때 비로소 resolve 또는 reject를 호출하는 방법으로 동기적 표현이 가능**하다.

- 비동기 작업의 동기적 표현(2) → Promise(2)

```jsx
var addCoffee = function (name) {
  return function (prevName) {
    return new Promise(function (resolve) {
      setTimeout(function () {
        var newName = prevName ? prevName + "," + name : name;
        console.log(newName);
        resolve(newName);
      }, 500);
    });
  };
};

addCoffee("에스프레소")()
  .then(addCoffee("아메리카노"))
  .then(addCoffee("카페모카"))
  .then(addCoffee("카페라떼"));

// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 에스프레소, 아메리카노, 카페모카, 카페라떼
```

Promise(1)의 예제에서 반복적인 부분을 함수화해 간결하게 표현할 수 있다.

- 비동기 작업의 동기적 표현(3) → Generator

```jsx
var addCoffee1 = function (prevName, name) {
  setTimeout(function () {
    coffeeMaker.next(prevName ? prevName + "," + name : name);
  }, 500);
};

var coffeeGenerator = function* () {
  var espresso = yield addCoffee1("", "에스프레소");
  console.log(espresso);
  var americano = yield addCoffee1(espresso, "아메리카노");
  console.log(americano);
  var mocha = yield addCoffee1(americano, "카페모카");
  console.log(mocha);
  var latte = yield addCoffee1(mocha, "카페라떼");
  console.log(latte);
};

var coffeeMaker = coffeeGenerator();

coffeeMaker.next();
// {value: undefined, done: false}
// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 에스프레소, 아메리카노, 카페모카, 카페라떼

coffeeMaker.next();
// {value: undefined, done: true}
```

Generator 함수를 실행하면 Iterator가 반환된다.

Iterator는 next라는 메서드를 갖고 있는데, 이 next 메서드를 통해 함수를 호출하면 함수 내부에 있는 yield에서 함수 실행을 멈추고, next 메서드를 다시 호출하면 멈췄던 부분에서부터 다음 yield에서 함수 실행을 멈춘다.

> 💡 **비동기 작업이 완료되는 시점마다 next 메서드를 호출해준다면 Generator 함수 내부의 소스가 순차적으로 진행**된다.

- 비동기 작업의 동기적 표현(4) → Promise + Asyne/await

```jsx
var addCoffee2 = function (name) {
  return new Promise(function (resolve) {
    setTimeout(function () {
      resolve(name);
    }, 500);
  });
};

var coffeeMaker2 = async function () {
  var coffeeList = "";
  var _addCoffee = async function (name) {
    coffeeList += (coffeeList ? "," : "") + (await addCoffee2(name));
  };

  await _addCoffee("에스프레소");
  console.log(coffeeList);
  await _addCoffee("아메리카노");
  console.log(coffeeList);
  await _addCoffee("카페모카");
  console.log(coffeeList);
  await _addCoffee("카페라떼");
  console.log(coffeeList);
};

coffeeMaker2();

// 에스프레소
// 에스프레소, 아메리카노
// 에스프레소, 아메리카노, 카페모카
// 에스프레소, 아메리카노, 카페모카, 카페라떼
```

비동기 작업을 수행하고자 하는 함수 앞에 **async**를 표기

함수 내부에서 실질적인 비동기 작업이필요한 위치마다 **await** 표기

> 💡 뒤의 내용을 Promise로 자동 전환하고, 해당 내용이 resolve 된 이후에야 다음으로 진행한다.(then과 흡사)

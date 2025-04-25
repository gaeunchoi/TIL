# 1️⃣ 클로저의 의미 및 원리 이해

## 1-1. 의미

> 클로저란 어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우, A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상

## 1-2. 이해

MDN(Mozilla Developer Network)에서는 클로저를 “함수와 그 함수가 선언될 당시의 lexical environment의 상호관계에 따른 현상”이라고 소개했다.

- 선언될 당시의 lexical environment: 실행 컨텍스트의 구성 요소 중 하나인 outerEnvironmentReference에 해당
- LexicalEnvironment의 environmentRecord와 outerEnvironmentReference에 의해 변수의 유효범위인 스코프가 결정되고, 스코프 체인이 가능

> 즉, 어떤 컨텍스트 A에서 선언한 내부 함수 B의 실행 컨텍스트가 활성화 된 시점에서는 B의 outerEnvironmentReference가 참조하는 대상인 A의 LexicalEnvironment에도 접근 가능

## 1-3. 예제

```jsx
function outer() {
  let count = 0;
  var inner = function () {
    return ++count;
  };
  return inner;
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

- `outer` 함수가 실행되면 내부의 `count` 변수를 가지고 있는 `inner` 함수 리턴
- `counter`는 `inner`를 가리키게 되고, `count` 변수는 `outer` 실행 컨텍스트가 종료되어도 사라지지 않음
- 왜? → `inner` 함수가 `count`에 접근하고 있기 때문에 메모리에 계속 남아있는다

<br>

# 2️⃣ 클로저와 메모리 관리

## 2-1. 클로저는 메모리 누수의 원인이 될 수 있다 ?

메모리 누수의 위험을 이유로 클로저 사용을 조심해야한다고 하지만, 메모리 소모는 클로저의 본질적인 특성이다.

오히려 이러한 특성을 정확히 이해하고 잘 활용하도록 해야한다.

## 2-2. 관리 방법

클로저는 어떤 필요에 의해 의도적으로 함수의 지역변수를 메모리를 소모하도록 함으로써 발생한다.

그렇다면, 그 필요성이 사라진 시점에는 더는 메모리를 소모하지 않게 해주면 된다.

→ 참조 카운트를 0으로 만들면 GC(Garbage Collector)의 수거대상이 된다는 말!

### 2-2-1. return에 의한 클로저 메모리 해제

```jsx
function outer() {
  let count = 0;
  var inner = function () {
    return ++count;
  };
  return inner;
}

console.log(outer());
console.log(outer());
outer = null; // 참조 해제
```

### 2-2-2. setInterval에 의한 클로저 메모리 해제

```jsx
(function () {
  let count = 0;
  var intervalId = null;
  var inner = function () {
    if (++count >= 10) {
      clearInterval(intervalId);
      inner = null; // inner 식별자의 함수 참조 해제
    }
    console.log(count);
  };
  intervalId = setInterval(inner, 1000);
})();
```

### 2-2-3. eventListener에 의한 클로저 메모리 해제

```jsx
(function () {
  var count = 0;
  var button = document.createElement("button");
  button.innerText = "click";

  var clickHandler = function () {
    console.log(++count, "times clicked");
    if (count >= 10) {
      button.removeEventListener("click", clickHandler);
      clickHandler = null;
    }
  };

  button.addEventListener("click", clickHandler);
  document.body.appendChild(button);
})();
```

<br>

# 3️⃣ 클로저 활용 사례

## 3-1. 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때

`alertFruitBuilder` 함수가 각 과일 이름을 기억하는 클로저를 반환하여, 각 li 요소의 클릭 이벤트 핸들러가 올바르게 과일 이름을 표시한다.

```jsx
var alertFruitBuilder = function (fruit) {
  return function () {
    alert("Your choice is " + fruit);
  };
};

var fruits = ["Apple", "Banana", "Cherry"];

fruits.forEach(function (fruit) {
  var $li = document.createElement("li"); // 새로운 li 요소 생성
  $li.innerText = fruit;
  $li.addEventListener("click", alertFruitBuilder(fruit));
  document.body.appendChild($li);
});
```

## 3-2. 접근 권한 제어(정보 은닉)

- 정보은닉: 어떤 모듈의 내부 로직에 대해 외부로의 노출을 최소화해 모듈간의 결합도를 낮추고 유연성을 높이고자 하는 것
- 클로저를 이용하면 함수 차원에서 `public`한 값과 `private`한 값을 구분하는 것이 가능

```jsx
var createCar = function () {
  var fuel = Math.ceil(Math.random() * 10 + 10);
  var power = Math.ceil(Math.random() * 3 + 2);
  var moved = 0;

  var publicMembers = {
    get moved() {
      return moved;
    },
    run: function () {
      var km = Math.ceil(Math.random() * 6);
      var wasteFuel = km / power;
      if (fuel < wasteFuel) {
        console.log("이동 불가");
        return;
      }
      fuel -= wasteFuel;
      moved += km;
      console.log(km + "km 이동 (총 " + moved + "km). 남은 연료: " + fuel);
    },
  };

  Object.freeze(publicMembers);
  return publicMembers;
};

var car = createCar();
car.run(); // 5km 이동(총 5km). 남은 연료 10.3333
console.log(car.moved); // 5
console.log(car.fuel); // undefined
console.log(car.power); // undefined

car.fuel = 1000;
console.log(car.fuel); // 1000
car.run(); // 5km 이동(총 10km). 남은 연료 8.6666

car.power = 100;
console.log(car.power); // 100
car.run(); // 3km 이동(총 13km). 남은 연료 7.6666

car.moved = 1000;
console.log(car.moved); // 13
car.run(); // 1km 이동(총 14km). 남은 연료 7.3333
```

### 💡 클로저로 접근권한 제어하는 방법

1. 함수에서 지역변수 및 내부함수 등을 생성
2. 외부에 접근권한을 주고자 하는 대상들로 구성된 참조형 데이터를 return한다.

## 3-3. 부분 적용 함수

`부분 적용 함수 partially applied function`: n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가, 나중에 (n-m)개의 인자를 넘기면 비로소 원래 함수의 실행 결과를 얻을 수 있게끔 하는 함수

### 3-3-1. bind 메서드를 활용한 부분 적용 함수

가변 인자를 받아 합을 계산하는 `add` 함수를 정의하고, 그 안에서 `arguments`를 통해 전달된 모든 인자의 합을 계산하고 결과를 리턴한다.

```jsx
var add = function () {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
};

// 부분 적용 함수를 생성, 첫 다섯 개의 인자(1, 2, 3, 4, 5)를 미리 적용
var addPartial = add.bind(null, 1, 2, 3, 4, 5);

// 나머지 인자(5, 6, 7, 8, 9, 10)를 전달하여 부분 적용 함수 호출
console.log(addPartial(5, 6, 7, 8, 9, 10)); // 출력: 55
```

`addPartial` : 인자 5개를 미리 적용하고, 추후 추가적으로 인자들을 전달하면 모든 인자를 모아 원래의 함수가 실행되는 **부분 적용 함수**이다.

### 3-3-2. 디바운스 함수를 이용한 부분 적용 함수

디바운스 함수는 사용자의 입력에 반응하는 이벤트 처리기에서 유용하게 사용된다.

```jsx
var debounce = function (eventName, func, wait) {
  var timeoutId = null;
  return function (event) {
    var self = this;
    console.log(eventName, "event발생");
    clearTimeout(timeoutId);
    timeoutId = setTimeout(func.bind(self, event), wait);
  };
};

var moveHandler = function (e) {
  console.log("move event 처리");
};

var wheelHandler = function (e) {
  console.log("wheel event 처리");
};

document.body.addEventListener("mouseover", debounce("move", moveHandler, 500));

document.body.addEventListener(
  "mousewheel",
  debounce("move", moveHandler, 500)
);
```

## 3-4. 커링 함수

- 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출될 수 있게 체인 형태로 구성한 것
- 커링은 한 번에 하나의 인자만 전달하는 것이 원칙
- 함수를 실행한 결과는 그 다음 인자를 받기 위해 대기만 하고, 마지막 인자가 전달되기 전까지 원본 함수 실행 x

```jsx
var curry3 = function (func) {
  return function (a) {
    return function (b) {
      return func(a, b);
    };
  };
};

// getMaxWith10: Math.max 함수에 첫 번째 인자를 10으로 고정한 부분 적용 함수 생성
var getMaxWith10 = curry3(Math.max)(10);

// getMinWith10: Math.min 함수에 첫 번째 인자를 10으로 고정한 부분 적용 함수 생성
var getMinWith10 = curry3(Math.min)(10);

console.log(getMaxWith10(8)); // 출력: 10 (10 > 8)
console.log(getMaxWith10(25)); // 출력: 25 (25 < 10)

console.log(getMinWith10(8)); // 출력: 8 (8 < 10)
console.log(getMinWith10(25)); // 출력: 10 (10 < 25)
```

- `getMaxWith10`: Math.max 함수에 첫 번째 인자를 10으로 고정한 부분 적용 함수 생성
- `getMinWith10`: Math.min 함수에 첫 번째 인자를 10으로 고정한 부분 적용 함수 생성
- `curry3`: 커링 함수 정의
- `function(a)`: 첫 번째 인자를 받아 두 번째 함수를 반환
- `function(b)`: 두 번째 인자를 받아 최종 함수 실행

## 1️⃣ 클래스와 인스턴스의 개념 이해

> **클래스**: 공통 요소를 지니는 집단을 분류하기 위한 개념
> **인스턴스**: 어떤 클래스의 속성을 지니는 실존하는 개체

코어자바스크립트 본 책에서는 ‘음식’이라는 범주로 클래스를 설명해준다.

음식이라는 **클래스**안에 고기, 채소, 과일 등등 다양한 ‘먹을 것’이라는 공통 요소들이 있고, 과일 밑에는 귤류와 같은 또 다른 요소가 들어올 수 있다. 그리고 귤류에 속하는 실제 개체를 **인스턴스**라고 한다.

위처럼 상위의 개념을 `상위 클래스 superclass`, 하위의 개념을 `하위 클래스 subclass`라고 하며

클래스는 하위로 갈수록 상위 클래스의 속성을 상속하면서 더 구체적인 요건이 추가 또는 변경된다.

<br>

## 2️⃣ 자바스크립트의 클래스

### 프로토타입 체이닝에 의한 ?

[코어자바스크립트 6장](https://velog.io/@gaeunchoi/JavaScript-%EC%BD%94%EC%96%B4%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-6%EC%9E%A5-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85)에서 자바스크립트는 프로토타입 기반 언어이므로 클래스의 개념이 존재하지 않는다고 언급한다. 여기서, 프로토타입을 일반적인 의미에서의 클래스 관점에서 접근해보면 비슷하게 해석할 수 있는 요소가 있다.

- 생성자 함수 Array를 new 연산자와 함께 호출하면 인스턴스가 생성
- 이 때 Array를 일종의 클래스라고 하면, Array의 prototype 객체 내부 요소들이 인스턴스에 **상속**된다고 볼 수 있음

> 💡 프로토타입 체이닝에 의한 참조지만 결과적으로는 클래스의 상속과 동일하게 동작하기 때문
> (내부 프로퍼티 중 prototype 프로퍼티를 제외한 나머지는 인스턴스에 상속되지 않는다.)

### 스태틱 메서드, 프로토타입 메서드

인스턴스가 상속(참조)되는지 여부에 따라 스태틱 멤버와 인스턴스 멤버로 나뉜다.

여느 클래스 기반 언어와 달리 자바스크립트에서는 인스턴스에서도 직접 메서드를 정의할 수 있기 때문에 **인스턴스 메서드** 라는 명칭보다는 **프로토타입 메서드**라고 부른다.

```jsx
var Rectangle = function (width, height) {
  this.width = width;
  this.height = height;
};

Rectangle.prototype.getArea = function () {
  return this.width * this.height;
};

Rectangle.isRectangle = function (instance) {
  return (
    instance instanceof Rectangle && instance.width > 0 && instance.height > 0
  );
};

var rect1 = new Rectangle(3, 4);
console.log(rect1.getArea()); // 12 (o)
console.log(rect1.isRectangle(rect1)); // Error (x)
console.log(Rectangle.isRectangle(rect1)); // true
```

- `console.log(rect1.Rectangle(rect1))`코드에서 rect1 인스턴스에서 isRectangle이라는 메서드에 접근하려고 할 때 에러가 발생하는 이유는 ?
- rect1.**proto**와 rect1.**proto**.**proto**(=Object.prototype)에서 검색을 했는데 해당 메서드를 찾지 못했기 때문이다.

> 💡 이렇게 인스턴스에서 직접 접근할 수 없는 메서드를 **스태틱 메서드**라고 한다.
> 스태틱 메서드는 생성자 함수를 this로 해야만 호출 가능하다.

<br>

## 3️⃣ 클래스 상속

> 💡 자바스크립트에서 클래스 상속을 구현했다 === 프로토타입 체이닝을 잘 연결한 것

클래스에 있는 값이 인스턴스의 동작에 영향을 주면 안된다. 인스턴스와의 관계에서는 구체적인 데이터를 지니지 않고 오직 인스턴스가 사용할 메서드만을 지니는 추상적인 툴로서만 작동하게끔 작성해야 예기치않은 오류 발생을 막을 수 있다.

### 클래스가 구체적인 데이터를 지니지 않게 하는 방법

1. 인스턴스 생성 후 프로퍼티 제거

- 일단 만들고 나서 프로퍼티들을 일일이 지우고 더는 새로운 프로퍼티를 추가할 수 없게 하는 것

```jsx
var extendClass1 = function (SuperClass, SubClass, subMethods) {
  SubClass.prototype = new SuperClass();
  for (var prop in Subclass.prototype) {
    if (SubClass.prototype.hasOwnProperty(prop)) {
      delete SubClass.prototype[prop];
    }
  }

  if (subMethods) {
    for (var method in subMethods) {
      SubClass.prototype[method] = subMethods[method];
    }
  }
  Object.freeze(SubClass.prototype);
  return SubClass;
};

var Square = extendClass1(Rectangle, function (width) {
  Rectangle.call(this, width, width);
});
```

2. 빈 함수(Bridge) 활용

- SubClass의 prototype에 직접 SuperClass의 인스턴스를 할당하는 대신
- 아무런 프로퍼티를 생성하지 않는 빈 생성자 함수(Bridge)를 하나 더 만들어서
- 그 prototype이 SuperClass의 prototype을 바라보게끔 한 다음
- SubClass의 prototype에는 Bridge의 인스턴스를 할당하게 함

```jsx
var extendClass2 = (function () {
  var Bridge = function () {};
  return function (SuperClass, SubClass, subMethods) {
    Bridge.prototype = SuperClass.prototype;
    SubClass.prototype = new Bridge();

    if (subMethods) {
      for (var method in subMethods) {
        SubClass.prototype[method] = subMethods[method];
      }
    }
    Object.freeze(SubClass.prototype);
    return SubClass;
  };
})();
```

3. Object.create 활용

- SubClass의 prototype의 **proto **가 SuperClass의 prototype을 바라보되
- SuperClass의 인스턴스가 되지는 않으므로 앞의 두 방법도 간단하면서 안전함

```jsx
// (...생략)
Square.prototype = Object.create(Rectangle.prototype);
Object.freeze(Square.prototype);
// (...생략)
```

### constructor 복구하기

- 위 세가지 방법 모두 기본적인 상속은 성공, 그러나 SubClass 인스턴스의 constructor는 여전히 SuperClass를 가리킨다.
- 이를 해결하기위해 위 코드들의 SubClass.prototype.constructor가 원래의 SubClass를 바라보도록 하게 하면 된다.

```jsx
SubClass.prototype.constructor = SubClass;
```

### 상위 클래스에의 접근 수단 제공

요긴 더 공부하고 정리할게요 !\_!…

<br>

## 4️⃣ ES6의 클래스 및 클래스 상속

ES6부터 클래스 문법이 도입된다 !

```jsx
var Rectangle = class {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  getArea() {
    return this.width * this.height;
  }
};

var Square = class extends Rectangle {
  constructor(width) {
    super(width, width);
  }
  getArea() {
    console.log("size is :", super.getArea());
  }
};
```

- class 명령어 뒤에 ‘extends Rectangle’ 이라는 내용을 추가함으로써 상속관계 설정이 된다.
- constructor 내부에서는 super라는 키워드를 함수처럼 사용할 수 있는데, 이 함수는 SuperClass의 constructor를 실행
- constructor 메서드를 제외한 다른 메서드에서는 **super** 키워드를 객체처럼 활용할 수 있고, 이 때 객체는 SuperClass.prototype을 바라보는데 호출한 메서드의 this는 super가 아닌 원래의 **this**를 따른다.

## 1️⃣ 동기 vs 비동기

`동기`는 직렬적으로 작동하는 방식이고, `비동기`는 병렬적으로 작동하는 것이다.  
다시말해, `비동기`란 특정 코드가 끝날때 까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 것이다.

JavaScript 엔진은 Single Thread라서 한 번에 하나의 작업만 수행할 수 있다.  
즉, 자바스크립트는 기본적으로 동기식 처리 모델이라는 것이다. 그래서 JS 엔진은 비동기 처리가 가능하도록 설계되었다.

### 1. 동기 Synchronous

- 직렬적으로 태스크 수행
- 요청을 보낸 후 응답을 받아야지만 다음 동작을 수행. 처리되지 않은 태스크가 있다면 나머지 태스크 대기
- 시스템의 전체적인 효율 저하

![synchronous.png](../img/sync,async/synchronous.png)
출처: [동기식 처리모델](<[https://poiemaweb.com/es6-promise](https://poiemaweb.com/es6-promise)>)

### 2. 비동기 Asynchronous

- 병렬적으로 태스크 수행
- 태스크가 종료되지 않아도 다음 작업 실행
- 비동기 요청 시 응답 후 처리할 `콜백함수`를 함께 알려준다. → 해당 태스크가 완료되었을 때 콜백함수 호출

![asynchronous.png](../img/sync,async/asynchronous.png)
출처: [비동기식 처리모델](<[https://poiemaweb.com/es6-promise](https://poiemaweb.com/es6-promise)>)

- 비동기 처리를 위해서 콜백 패턴을 사용 → 처리 순서를 보장하기 위해 여러개의 콜백 함수가 중첩되고 복잡도가 높아져 `콜백 헬(Callback Hell)`이 발생하는 단점이 있다.
- 콜백 헬은 가독성을 해치며, 실수를 유발하기 딱 좋다 ..

```jsx
first(function(v1){
	second(v1, function(v2){
		third(v2, function(v3){
			fourth(v3, function(v4){
				.....
			});
		});
	});
});
```

<br>

## 2️⃣ Promise

JavaScript에서 **비동기적으로 실행하는 작업의 결과(성공 resolve / 실패 reject)를 나타내는 `객체`**

### 1. Promise 호출 과정

```jsx
// 프로미스 객체를 반환하는 함수 생성
function myPromise() {
  return new Promise((resolve, reject) => {
    if (/* 성공 조건 */) {
      resolve(/* 결과 값 */);
    } else {
      reject(/* 에러 값 */);
    }
  });
}

// 프로미스 객체를 반환하는 함수 사용
myPromise()
    .then((result) => {
      // 성공 시 실행할 콜백 함수
    })
    .catch((error) => {
      // 실패 시 실행할 콜백 함수
    });
```

1. 비동기 함수 내에서 Promise 객체 생성 후 내부에서 비동기 처리 구현  
    이 때, Promise 생성자 안에 두 개의 매개 변수를 가진 콜백 함수(executor)를 넣게 되는데
   - 첫번째 인수: 작업이 성공했을 때 `성공 resolve`임을 알려주는 객체
   - 두번째 인수: 작업이 실패했을 때 `실패 reject`임을 알려주는 오류 객체
     <br>
2. 비동기 처리에 성공하면 `resolve` 메서드 호출  
   이 때, resolve 메서드의 인자로 비동기 처리 결과가 전달되는데, 이 결과는 Promise 객체의 후속 처리 메서드인 `then()`으로 전달된다.
   <br>

3. 비동기 처리에 실패하면 `reject` 메서드 호출  
   이 때, reject 메서드의 인자로 에러 메세지를 전달하고, 이 메세지는 Promise 객체의 후속 처리 메서드인 `catch()`로 전달된다
   <br>

4. 비동기 처리의 성공/실패 여부와 상관없이 `finally` 메서드 호출  
   프로미스가 이행되거나 거부될 때 상관없이 실행할 콜백 함수를 등록하고 새로운 프로미스를 반환한다.

### 2. Promise Chaining 체이닝

`then` 메서드는 promise 객체를 리턴하고 인수로 받은 콜백 함수들의 리턴값을 계속해서 이어 받는다.  
→ `promise chaining`은 promise 핸들러를 연달아 연결하는 것을 의미한다.

```jsx
const 장보기 = new Promise((resolve) => {
  setTimeout(() => {
    resolve("🥚 계란 구매 완료!");
  }, 1000);
});

장보기
  .then((result) => {
    console.log(result);
    return "🍜 라면 끓이기 시작"; // 다음 작업으로 전달
  })
  .then((result) => {
    console.log(result);
    return "🍳 계란 넣기"; // 다음 작업으로 전달
  })
  .then((result) => {
    console.log(result);
    console.log("🎉 완성!");
  });
```

<br>

## 3️⃣ Async / Await

async와 await는 자바스크립트의 비동기 처리 패턴 중 가장 최근에 나온 문법이다.

위에서 본 콜백함수와 프로미스의 단점을 보완하고, 가독성 좋은 코드를 작성할 수 있게 도와준다.

```jsx
async function 함수() {
  await 비동기처리_메서드();
}
```

### 1. async

`async` 키워드를 function 앞에 사용하면 해당 함수는 항상 Promise를 반환한다.  
Promise가 아닌 값을 반환하더라도 이행 상태의 Promise(resolved promise)를 값을 감싸 반환한다.

→ 그냥 간단하게 생각하면 await를 사용하기 위한 선언문으로 봐도 된다.

```jsx
// ✅ 함수 선언식
async function func1() {
  const res = await fetch(url); // 요청을 기다림
  const data = await res.json(); // 응답을 JSON으로 파싱
}
func1();

// ✅ 함수 표현식
const func2 = async () => {
  const res = await fetch(url); // 요청을 기다림
  const data = await res.json(); // 응답을 JSON으로 파싱
};
func2();
```

### 2. await

`await`는 async 함수 안에서만 동작한다.  
이는, Promise가 처리될 때까지 기다리는 역할을 한다. 그리고 결과는 그 이후에 반환된다.

```jsx
async function func() {
  const res = await fetch(url); // 요청을 기다림
  const data = await res.json(); // 응답을 JSON으로 파싱
  // data 처리
  console.log(data);
}
func();
```

### 3. async / await 에러 제어

`await`가 던진 에러는 `try...catch` 를 이용해서 잡을 수 있다.

```jsx
async function func() {
  try {
    const res = await fetch(url); // 요청을 기다림
    const data = await res.json(); // 응답을 JSON으로 파싱
    // data 처리
    console.log(data);
  } catch (err) {
    // 에러 처리
    console.error(err);
  }
}
func();
```

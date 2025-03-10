# JavaScript 정리 - 1

## 👀 변수

`변수 variable`: 데이터를 저장할 때 쓰이는 **이름이 붙은 저장소**

1. `let`  
   변할 수 있는 변수를 선언할 때 사용한다.

   `let`으로 선언한 변수에 저장한 값은 다른값으로 얼마든지 변경이 가능하고, 다른 변수에 복사도 가능하다.

   ```jsx
   let msg = "gaanii";
   msg = "myName";

   let another;
   another = msg;
   ```

   ⚠️ 같은 변수를 두 번 선언하면 에러가 발생한다.

   ```jsx
   let msg = "gaanii";
   let msg = "myName"; // SyntaxError: 'message' has already been declared
   ```

   <br>

2. `const`  
   변하지 않는 변수를 선언할 때 사용한다. 이 때, `const`로 선언한 변수를 `상수`라고 한다.

   ```jsx
   const text = "악깡버";
   ```

   ⚠️ 상수는 **재할당이 불가능**하다.  
   → 즉, 이미 값을 할당한 변수에 새로운 값을 할당하려고 하면 에러가 발생한다.  
   `const`를 사용한 것은 변수의 값이 절대로 변경되지 않는 것을 확신하는것 !

   ```jsx
   const text = "악깡버";
   text = "버티기 실패"; // error: 재할당 불가능
   ```

   <br>

3. 대문자 상수  
   기억하기 힘든 값(소수점 와랄라 이어지는 숫자, 색상 코드 등)을 변수에 할당할 때 별칭으로 사용하는 변수

   보통 `대문자` 와 `밑줄(_)`로 구성된 이름으로 변수명을 명명한다.

   ```jsx
   const COLOR_RED = "#F00";
   const COLOR_GREEN = "#0F0";
   const COLOR_BLUE = "#00F";

   // 색상을 고르고 싶을 때 별칭을 사용할 수 있게 되었습니다.
   let color = COLOR_GREEN;
   alert(color); // #0F0
   ```

   ❓ `일반 상수` vs `대문자 상수` 사용?

   - `일반 상수`: 코드가 실행된 이후 계산되어 할당되는 변하지 않는 상수
   - `대문자 상수`: 코드가 실행되기 전에도 이미 그 값을 알고 있는(하드코딩 해주는) 상수

<br>

4. 변수 명명 규칙

   - 변수명에는 `문자`, `숫자`, `기호 중에는 $, _` 만 들어갈 수 있다.
   - 첫 글자는 숫자가 들어갈 수 없다.
   - 같은 변수명이어도 대소문자를 구별한다.
   - [예약어 목록](<[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#reserved_words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#reserved_words)>)에 있는 단어는 사용할 수 없다.
   - [카멜케이스](<[https://en.wikipedia.org/wiki/Camel_case](https://en.wikipedia.org/wiki/Camel_case)>)가 사용된다.
   - 변수가 담고 있는 것이 무엇인지 잘 설명할 수 있는 이름이어야 한다.

   ```jsx
   // ✅ 올바른 변수명
   let $test;
   let _test;
   let testData;
   let test123;

   // ❌ 잘못된 변수명
   let 1test;
   let my-test;
   ```

<br>

## 👀 자료형

1. `숫자형 Number Type`

- 일반 숫자값: 1, 2, 3, …
- 특수 숫자값: Infinity(무한대), -Infinity, Nan(계산 중 에러 발생했다는 걸 나타내는 값)

<br>

2. `문자형 String Type`  
   큰 따옴표, 작은 따옴표 모두 사용 가능하고, 백틱(`) 을 사용하면 문자열 안에 변수를 넣을 수 있다.

```jsx
let str1 = "예시를 보여주마";
let str2 = "작은 따옴표도 괜찮다";
let phrase = `백틱 사용도 가능하다 ${str1}`;
```

<br>

3. `불린형 Boolean Type`  
   `true(참)` / `false(거짓)` 두 가지 값뿐인 자료형

<br>

4. `null`  
   오로지 null값만 포함하는 별도의 자료형을 만든다.

<br>

5. `undefined`  
   변수는 선언했는데 값을 할당하지 않았다면 자동으로 `undefined` 할당

```jsx
let test;
console.log(test); // undefined 출력
```

<br>

6. `객체 Object` & `심볼 Symbol`  
   데이터 컬렉션이나 복잡한 개체를 표현할 수 있다. → 추후 설명 덧붙일게오 !
   <br>

## 👀 연산자

1. 기본 연산자  
   `덧셈 +`, `뺄셈 -`, `곱셈 *`, `나눗셈 /`, `나머지 %`, `거듭제곱 **` 이 있다.

```jsx
let a = 7;
let b = 5;

console.log(a + b); // 덧셈(output: 12)
console.log(a - b); // 뺄셈(output: 2)
console.log(a * b); // 곱셈(output: 35)
console.log(a / b); // 나눗셈(output: 1.4)
console.log(a % b); // 나머지(output: 2)
console.log(a ** b); // 거듭제곱(output: 16807)
```

<br>

2. 증가 / 감소 연산자  
   `증가 ++`, `감소 --` 가 있다. 전위 / 후위 연산은 [Hayoung’s velog](<[https://velog.io/@iamhayoung/JavaScript-증감-연산자-Feat.-전위-연산자-후위-연산자](https://velog.io/@iamhayoung/JavaScript-%EC%A6%9D%EA%B0%90-%EC%97%B0%EC%82%B0%EC%9E%90-Feat.-%EC%A0%84%EC%9C%84-%EC%97%B0%EC%82%B0%EC%9E%90-%ED%9B%84%EC%9C%84-%EC%97%B0%EC%82%B0%EC%9E%90)>)에 정리가 잘 되어있다.

```jsx
let cnt = 3;
let num1 = cnt++;
console.log(num1); // 3
console.log(cnt); // 4

let num2 = cnt--;
console.log(num2); // 4
console.log(cnt); // 3
```

<br>

3. 비교 연산자

```jsx
console.log(2 > 1); // true
console.log(2 == 1); // false

console.log(2 == "2"); // true
console.log(2 == 2); // true

console.log(2 === "2"); // false
console.log(2 === 2); // true

console.log(2 != "2"); // false
console.log(2 !== "2"); // true

console.log(true == 1); // true
console.log(true === 1); // false

console.log(null == undefined); // true
console.log(null === undefined); // false

// == : 동등 연산자
// === : 일치 연산자(엄격한 동등 연산자, 자료형의 동등 여부까지 검사함)
```

연산자는 `===, !==` 로 일치 연산자를 사용하여 형 변환 없이 엄격하게 연산해야 에러 발생 확률을 줄여준다.
<br>

4. 논리 연산자

- `|| (OR)` : 둘 중 하나라도 true면 true, 그렇지 않으면 false

```jsx
console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

✅ 자바스크립트가 제공하는 OR의 추가 기능 = `단락 평가`

OR 연산자와 여러 개의 피연산자가 있을 때 왼쪽부터 truthy한지 판별하는데,

만약 truthy 하다면 해당 피연산자의 불린형으로 변환하기 전 원래 값을 반환

```jsx
console.log(1 || 0); // 1

let nameA = "";
let nameB = "";
let nameC = "C";
console.log(nameA || nameB || nameC); // C

let nameA = "";
let nameB = "";
let nameC = "";
console.log(nameA || nameB || nameC || "셋다 이름 빈 문자열임"); // 셋다 이름 빈 문자열임

let nameA = undefined;
let nameB = "BBB";
let nameC = "CCC";
console.log(nameA || nameB || nameC); // BBB -> truthy한 값을 만나면 나머지는 평가하지 않은채 멈춤

let nameA = null;
let nameB = "BBB";
console.log(nameA || nameB); // BBB

let nameA = undefined;
let nameB = null;
console.log(nameA || nameB || 0); // 0 -> 셋다 falsy하기 때문에 그럴 땐 마지막 값 반환
```

- `&& (and)` : 둘 중 하나라도 false 면 무조건 false 반환

```jsx
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false
```

- `! (NOT)` : 피연산자를 불린형으로 변환하고, 해당 불린형의 역을 반환

```jsx
console.log(!true); // false
console.log(!0); // true
```

<br>

5. ?? nullish 병합 연산자  
   `a ?? b` → a가 `null`도 아니고 `undefined`도 아니면 a값 반환 / 그 외의 경우 b 반환

⚠️ `??`와 `||`의 차이  
`||` 연산자는 첫 번째 truthy 값을 반환, `??` 연산자는 첫 번째 정의된 (즉, null이나 undefined가 아닌) 값을 반환합니다.

```jsx
// 예시
let height = 0

// 0은 fasly하므로 해당 콘솔은 100을 반환합니다.
console.log(height || 100)

// height가 null,undefined가 아닌 0이라는 값이 할당되었기 때문에 0이 출력됩니다.
console.log(height ?? 100)

---

// 만약, height 값이 undefined 라면?
let height; // 아무 값도 할당하지 않았으므로 undefined

// height가 undefined 이므로 100 출력됨
console.log(height ?? 100)
```

<br>

## 👀 조건문

1. `if` : 조건문에 뭐가 들어가던 무조건`불린값`으로 변환하여 반환함 → `true/false`만 나온다는 말

   ```jsx
   if (condition) {
     // 코드블럭 실행
   }
   ```

   <br>

2. `if-else`

   ```jsx
   if (condition1) {
     // condition1이 true이면 해당 코드블럭 실행
   } else if (condition2) {
     // condition2가 true이면 해당 코드블럭 실행
   } else {
     // condition 1, 2가 전부 false라면 해당 코드블럭 실행
   }
   ```

   <br>

3. `삼항 연산자` : `조건문 ? 참일 경우 실행 : 거짓일 경우 실행` 구조로 작성

   ```jsx
   let tmp = age > 18 ? true : false;
   ```

   <br>

## 👀 반복문

1. `while`문: condition이 참인 동안 반복문 내부 코드 실행  
   조건문을 활용하지 않고 반복문을 탈출하는 방법 → `break` 사용

```jsx
while (condition) {
  // condition(조건)이 truthy이면 반복문 본문 코드 실행
}
```

<br>

2. `for`문

- `begin`: 반복문 초기값 선언
- `condition`: 반복마다 해당 조건 확인, false이면 반복문 멈춤
- `step`: 각 반복 조건 확인 후 코드블럭 실행후 실행, 보통 증감문 사용
- 반복문을 돌다가 해당 단계는 스킵하고 싶다면 → `continue` 사용

```jsx
for (begin; condition; step) {
  // condition truthy 하다면 반복문 실행
}
```

<br>

3. `switch`문  
   각 case에 break를 걸지 않으면 아래 case를 모두 실행하게 된다.

```jsx
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    break;

  case 'value2':  // if (x === 'value2')
    ...
    break;

  default:
    ...
    break;
}
```

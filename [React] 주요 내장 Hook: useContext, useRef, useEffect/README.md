React에서 자주 사용되는 내장 Hook들이 있는데, 이전에 상태관리 Hook으로 `useState`, `useReducer`등을 소개했고 그 외에 자주 사용되는 세 가지 내장 Hook을 소개하겠다.

# 1️⃣ useContext

## 개념

`useContext`는 컴포넌트에서 Context를 읽고 구독할 수 있는 React Hook이라고 한다.([공식문서](https://ko.react.dev/reference/react/useContext))

Context API와 함께 사용되어 컴포넌트 트리 깊숙한 곳까지 데이터를 쉽게 전달할 수 있게 한다.

전역 테마, 로그인 정보, 다국어 설정 등 전역 상태가 필요한 경우에 자주 사용된다.

## 기본 사용법

전역 테마 설정을 예시로 보자!

1. Context 생성

`createContext`를 사용해 Context를 생성한다

```jsx
// ThemeContext.js
import React, { createContext, useContext } from "react";
const ThemeContext = createContext("light");
```

2. Provider로 값 제공

`Provider`를 사용해 Context 값을 제공한다

```jsx
// App.jsx
import { ThemeContext } from "./ThemeContext";
import MyChildComp from "./MyChildComp";

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <MyChildComp />
    </ThemeContext.Provider>
  );
}
```

3. useContext로 값 소비

하위 컴포넌트에서 `useContext`를 사용해 Context 값을 읽는다

```jsx
function MyChildComp() {
  const theme = useContext(ThemeContext);
  return <div>현재 테마는 {theme}입니다.</div>;
}
```

## 정리

- 전역 상태 공유에 유용
- 상태를 props로 넘기지 않아도 되기 때문에 props drilling 해결
- 리렌더링 자동 처리
- 상태 관리(`useState`, `useReducer`)를 함께 사용하여 더 유연한 전역 상태 관리 가능

<br>
<br>

# 2️⃣ useRef

## 개념

`useRef`는 리액트 훅의 한 종류로, Ref는 reference(참조)의 줄임말이다.

- DOM 요소에 직접 접근
- 렌더링 사이 값을 기억하되, 리렌더링을 유발하지 않음

## 기본 사용법

`myRef.current`를 통해 값에 접근하거나 수정하며, 값을 바꿔도 컴포넌트는 리렌더링되지 않는다.

```jsx
const myRef = useRef(초기값);
```

1. DOM 접근 예시

```jsx
function MyInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>포커스</button>
    </>
  );
}
```

2. 값 저장(기억) 예시

```jsx
function Timer() {
  const cntRef = useRef(0);

  const increment = () => {
    cntRef.current += 1;
    console.log("현재 cnt:", cntRef.current);
  };

  return <button onClick={increment}>증가</button>;
}
```

## 정리

- DOM 직접 제어 가능(`ref.current`)
- 컴포넌트가 리렌더링 될 때도 값을 쥬이할 수 있음
- 상태 변화가 필요 없지만, 값을 기억해야 할 때 유용

<br>
<br>

# 3️⃣ useEffect

## 개념

`useEffect`는 컴포넌트가 마운 / 업데이트 / 언마운트될 때 특정 작업을 수행할 수 있게 해주는 Hook이다.

## 기본 사용법

```jsx
useEffect(() => {
	// 실행 로직
}, [의존성 배열]);
```

1. 의존성 배열이 빈 배열이면 → 마운트 시 한번만 실행

```jsx
useEffect(() => {
  console.log("컴포넌트 마운트");
  return () => {
    console.log("컴포넌트 언마운트");
  };
}, []);
```

2. 의존성 배열에 특정 값이 들어 있으면 → 그 값이 바뀔 때마다 실행

```jsx
useEffect(() => {
  console.log("cnt가 변경됨");
}, [cnt]);
```

3. 의존성 배열이 없으면 → 모든 렌더링마다 실행

의존성 배열을 생략하면 모든 리렌더링마다 실행되므로, 네트워크 요청이나 무거운 연산이 있다면 불필요하게 여러번 실행될 수 있으니 되도록이면 의존성 배열을 명시하도록 !

```jsx
useEffect(() => {
  console.log("useEffect 렌더링마다 실행");
});
```

## 정리

- 생명주기 제어 가능(마운트, 업데이트, 언마운트)
- 의존정 배열로 제어 범위 지정
- fetch, 타이머, 구독 등 다양한 곳에 사용

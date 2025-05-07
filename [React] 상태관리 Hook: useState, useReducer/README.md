React에서 컴포넌트의 상태를 관리할 때 가장 많이 사용하는 두 가지 Hook을 정리 !

## 1️⃣ useState

### 개념

- 함수형 컴포넌트에 상태(state)를 추가할 수 있게 해주는 가장 기본적인 Hook
- 한 컴포넌트에서 하나의 값 또는 단순한 구조의 상태를 다루기에 적합
- 상태의 타입은 숫자, 문자열, 불리언, 배열, 객체 등 JavaScript의 모든 값이 가능
- 상태 변경 함수(`setState`)를 호출하면 해당 컴포넌트가 리렌더링되어 새로운 상태값이 반영

### 코드 예시

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // 초기값 0

  const increment = () => setCount((prev) => prev + 1);

  return (
    <>
      <h2>Count: {count}</h2>
      <button onClick={increment}>+1</button>
    </>
  );
}
```

### 내부 동작

- useState는 배열을 반환하며, `[현재 상태, 상태 변경 함수]` 형태로 구조 분해
- 상태 변경 함수(`setState`)는 비동기적으로 동작하며, 새로운 값을 직접 넣거나 함수형 업데이트로 이전 상태를 기반으로 변경 가능
- 초기값은 첫 렌더링 시에만 적용되며, 함수형 초기화도 가능(ex: `useState(() => expensiveComputation())`)
- 여러 개의 상태가 필요하다면 `useState`를 여러 번 호출도 가능. 단, 서로 의존적인 상태가 많아지면 관리 어려움

## 2️⃣ useReducer

### 개념

- 상태가 여러 값(객체, 배열 등)으로 구성되거나, 상태 전이 로직이 복잡할 때 사용하는 Hook
- 상태 변경 로직을 컴포넌트 바깥의 “리듀서 함수”로 분리하여, 액션(action)에 따라 상태를 체계적으로 관리
- Redux의 reducer 패턴과 유사하게, `state + action = newState` 구조를 따름
- 상태와 함께 `dispatch` 함수를 반환하며, `dispatch(action)`을 호출하면 리듀서가 실행되어 새로운 상태를 계산

### 코드 예시

```jsx
import { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      <h2>Count: {state.count}</h2>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+1</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-1</button>
    </>
  );
}
```

### 내부 동작

- `useReducer(reducer, initialState)`는 `상태, dispatch`를 반환
- `dispatch(action)`을 호출하면 리듀서 함수가 `(현재 상태, action)`을 받아 새로운 상태를 반환
- 리듀서는 순수 함수로, 같은 입력에 항상 같은 출력을 냄
- 상태 변경 규칙을 한 곳에 모아두기 때문에, 복잡한 상태 관리나 상태 전이 추적이 쉬워짐

### useState vs. useReducer

| 항목              | `useState`                                  | `useReducer`                                 |
| ----------------- | ------------------------------------------- | -------------------------------------------- |
| 상태 구조         | 단순함(숫자, 문자열, 불리언, 배열, 객체 등) | 복잡함(여러값, 객체, 배열, 상태간 의존성 등) |
| 업데이트 방법     | 직접 값 변경 또는 함수형 업데이트           | 액션 기반 dispatch                           |
| 로직 위치         | 컴포넌트 내부                               | 리듀서 함수로 구조화(컴포넌트 외부)          |
| 사용 상황         | 간단한 상태, 독립적인 값                    | 복잡한 상태, 상태 전이 규칙이 필요한 경우    |
| 예시              | 카운터, 단일 입력값                         | 폼 상태, 장바구니, 비동기 데이터 처리        |
| 확장성/유지보수성 | 상태가 많아지면 복잡해짐                    | 상태 전이 관리가 명확, 확장/유지보수 용이    |

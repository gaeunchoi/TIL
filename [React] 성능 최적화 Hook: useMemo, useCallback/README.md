# 1️⃣ useMemo

## 개념

`useMemo`는 렌더링 과정 중 발생하는 무거운 계산 작업의 **연산 결과(값)**을 메모이제이션해, 불필요한 연산을 방지한다.

```jsx
const memoizedValue = useMemo(() => {
  // 엄청 복잡한 계산
  return 계산된값;
}, [의존성]);
```

- 리렌더링할 때마다 값을 재계산하지 않아도 되는 경우
- 무거운 연산을 매번 다시 하지 않도록 방지하고 싶을 때

## 잘못된 예시

```jsx
const value = useMemo(() => count + 1, [count]);
```

`count + 1`이라는 가벼운 연산이므로 `useMemo`를 사용하는게 오히려 메모리 낭비일 수 있다.

무거운 계산이 아니라면, 그냥 일반 변수로 계산하는걸 추천한다 !

## 예시

```jsx
import { useMemo, useState } from "react";

function ExpensiveComponent({ num }) {
  const [count, setCount] = useState(0);

  const expensiveResult = useMemo(() => {
    console.log("== 복잡한 계산 중 ==");
    let result = 0;
    for (let i = 0; i < 1e9; i++) result += i;
    return result + num;
  }, [num]);

  return (
    <div>
      <p>계산 결과: {expensiveResult}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

위 예시를 봤을 때, `num`이 바뀌지 않으면 `expensiveResult`는 재계산되지 않는다.

`setCount`를 눌러도 무거운 연산이 다시 일어나지 않기 때문에 성능 향상이 된다.

<br />
<br />

# 2️⃣ useCallback

## 개념

`useCallback`은 매 렌더링마다 함수 인스턴스가 새로 만들어지는 것을 막기 위한 Hook이다.

함수도 일종의 객체이므로, 부모 컴포넌트에서 자식 컴포넌트로 props로 함수를 넘기면 참조값이달라져 자식이 리렌더링될 수 있다.

```jsx
const memoizedCallback = useCallback(() => {
  // 함수안에 작업 와랄라
}, [의존성]);
```

- 자식 컴포넌트에 콜백 함수를 props로 넘길 때
- 렌더링마다 함수가 새로 생성되는 걸 막고 싶을 때
- `React.memo`와 함께 사용하면 효과적

## 예시

```jsx
import { useCallback, useState } from "react";

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("클릭되었음");
  }, []); // 한 번만 생성됨

  return (
    <div>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>카운트 증가</button>
    </div>
  );
}

const Child = React.memo(({ onClick }) => {
  console.log("자식 컴포넌트 렌더링");
  return <button onClick={onClick}>클릭</button>;
});
```

위 예시를 봤을 때, `Parent`가 리렌더링될 때 `handleClick` 함수가 새로 만들어지지 않기 때문에, `Child`도 다시 렌더링되지 않는다.

# 정리

| Hook          | 메모이제이션 대상 | 주요 사용 목적                        |
| ------------- | ----------------- | ------------------------------------- |
| `useMemo`     | 계산된 **값**     | 무거운 연산 방지                      |
| `useCallback` | **함수**          | 함수 재생성 방지, `React.memo`와 궁합 |

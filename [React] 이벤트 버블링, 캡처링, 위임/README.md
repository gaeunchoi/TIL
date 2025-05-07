React에서 DOM 이벤트를 다룰 때 `이벤트 버블링`, `이벤트 캡처링`, `이벤트 위임` 등 개념들이 언급된다.

이 개념들은 브라우저의 이벤트 전파 Event Propagation 매커니즘을 기반으로 하며, React에서도 유사하게 동작한다.

# 📌 이벤트 전파 Event Propagation

브라우저에서 발생한 이벤트는 두 단계를 거쳐 전파가 이루어진다.

1. 캡처링 단계 Capturing Phase
   최상위 요소에서 이벤트 타겟까지 내려가는 단계
2. 버블링 단계 Bubbling Phase
   이벤트가 실제 타겟에서 발생한 후, 다시 상위 요소로 올라가는 단계

```console
[캡처링 단계] window → html → body → div → button
[버블링 단계] button → div → body → html → window
```

> 💡 React의 대부분의 이벤트는 **버블링 단계**에서만 감지된다.

<br>

# 📌 이벤트 버블링 Event Bubbling

React에서는 기본적으로 이벤트가 버블링된다. 아래 예시를 통해 이해를 해보자.

```jsx
function App() {
  const handleParentClick = () => console.log("👵🏻 Parent Clicked");
  const handleChildClick = () => console.log("👶🏻 Child Clicked");

  return (
    <div onClick={handleParentClick}>
      <button onClick={handleChildClick}>Click me!</button>
    </div>
  );
}
```

여기서 `Click me!` 버튼을 클릭하면 콘솔에 다음과 같은 결과가 출력된다.

```jsx
👶🏻 Child Clicked
👵🏻 Parent Clicked
```

> 💡 즉, 자식 요소인 button을 클릭하면 **이벤트가 버블링**되어 부모인 div까지 전달된다.

## ⛔️ 이벤트 버블링 방지: `e.stopPropagation()`

```jsx
const handleChildClick = (e) => {
  e.stopPropagation();
  console.log("이벤트 버블링 방지");
};
```

<br>

# 📌 이벤트 캡처링 Event Capturing

React에서 `onClickCpature`처럼 캡처링 단계에서 이벤트 감지도 가능하다

```jsx
<div onClickCapture={() => console.log("👵🏻 부모(캡처)")}>
  <button onClick={() => console.log("👶🏻 자식(버블)")}>Click me!</button>
</div>
```

여기서 `Click me!` 버튼을 클릭하면 콘솔에 다음과 같은 결과가 출력된다.

```jsx
👵🏻 부모(캡처)
👶🏻 자식(버블)
```

> 💡 이벤트 캡처링은 **이벤트가 내려오는 도중 감지**하는 것이다.

<br>

# 📌 이벤트 위임 Event Delegation

이벤트 위임이란 부모 요소에 이벤트를 등록해 자신의 이벤트를 감지하는 방법이다.

React는 내부적으로 이벤트를 루트(root)에서 위임하여 관리한다.

```jsx
function List() {
  const handleClick = (e) => {
    if (e.target.tagName === "LI") {
      console.log(`✅ ${e.target.textContent} 클릭됨`);
    }
  };

  return (
    <ul onClick={handleClick}>
      <li>Apple</li>
      <li>Banana</li>
      <li>Cherry</li>
    </ul>
  );
}
```

위 코드에서, 각 `<li>` 에 직접 `onClick`을 붙이지 않아도 동작한다.

<br>

# 💡 정리

| 개념          | 설명                                                       |
| ------------- | ---------------------------------------------------------- |
| 이벤트 버블링 | 이벤트가 타깃에서 부모로 올라가는 전파 방식                |
| 이벤트 캡처링 | 이벤트가 부모에서 타깃으로 내려가는 전파 방식              |
| 이벤트 위임   | 상위 요소에서 이벤트를 붙여 하위 요소의 이벤트를 일괄 처리 |

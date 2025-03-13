## 1️⃣ DOM이란

`DOM Document Object Model`: HTML, XML 문서를 객체 기반의 트리 구조로 표현한 프로그래밍 인터페이스이다.

즉, 문서의 구조화된 표현을 제공하며 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하여 그들이 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다.

<br>

## 2️⃣ DOM 접근 방법

### 1. querySelector 이용

- `querySelector`: CSS 선택자에 해당하는 가장 첫번째 DOM element 선택
- `querySelectorAll`: CSS 선택자에 해당하는 모든 DOM element 선택

### 2. ID, Class, Tag 이용

- `getElementById`: 주어진 문자열과 일치하는 id 값을 가진 요소 선택
- `getElementByClassName`: 주어진 클래스 이름을 가진 모든 자식 요소를 배열과 같은 객체로 반환

### 3. 부모, 형제, 자식 DOM 요소 선택

- `parentElement`: 호출된 element의 부모 요소 반환
- `nextElementSibling`: 호출된 element의 바로 뒤에 있는 요소(형제) 반환
- `previousElementSibling`: 호출된 element의 바로 앞에 있는 요소(형제)를 반환

### [참고] getElementBy vs. querySelectorAll()

`getElementBy*`: 문서에 변경이 있을 때마다 자동으로 갱신되어 최신 상태를 유지한다.

`querySelectorAll()`: 정적인 컬렉션을 반환해 자동으로 갱신이 안된다

```jsx
// getElementsBy~ 는 살아있는 컬렉션을 반환한다.
<div>첫 번째 div</div>
<script>
  let divs = document.getElementsByTagName('div');
  alert(divs.length); // 1
</script>
<div>두 번째 div</div>

<script>
  alert(divs.length); // 2
</script>

// querySelectorAll 은 정적인 컬렉션을 반환함.
<div>첫 번째 div</div>
<script>
  let divs = document.querySelectorAll('div');
  alert(divs.length); // 1
</script>
<div>두 번째 div</div>

// 문서에 새로운 div가 추가되었지만 여전히 1을 반환하고 새로운 div를 반영하지 못하고 있다.
<script>
  alert(divs.length); // 1
</script>
```

<br>

## 3️⃣ DOM 제어 방법

### 1. 요소 생성

매개변수로 주어진 tagName의 HTML 요소를 만들어 반환한다.

```jsx
let element = document.creageElement("tagName");
```

### 2. add, remove, toggle로 class 제어

- `classList.add`: 지정한 클래스 추가

```jsx
let addClass = element.classList.add("className");
```

- `classList.remove`: 지정한 클래스 삭제

```jsx
let removeClass = element.classList.remove("className");
```

- `classList.toggle`: 클래스가 있다면 제거, 클래스가 없다면 추가

```jsx
let toggleClass = element.classList.toggle("className");
```

### 3. 속성 추가

`setAttribute` 를 이용해 지정된 속성을 요소에 추가하고 지정된 값 제공

- attribute: 속성의 특정 이름 설정
- value: 속성에 값 할당

```jsx
element.setAttribute("attribute", "value");
```

### 4. 요소 추가

`appendChild()`를 이용해 특정 부모 노드의 자식 노드 리스트 중 마지막 자식으로 추가

- element: 부모 요소
- aChild: 위 부모 밑으로 들어갈 자식 요소

```jsx
element.appendChild(aChild);
```

<br>

## 4️⃣ 이벤트와 이벤트 리스너

`이벤트 Event` 객체란 DOM 내에 위치한 이벤트를 나타내며, 사용자가 현재 취한 액션에 대한 정보를 갖고 있다.

이 이벤트를 받고 처리하기 위해 `addEventListener` 메서드를 이용한다.

```jsx
const btn = document.createElement("button");
btn.textContent = "click me";
document.body.appendChild(btn);

btn.addEventListener("click", function () {
  alert("clicked button");
});
```

### 1. 마우스 이벤트

- `click` – 요소 위에서 마우스 왼쪽 버튼을 눌렀을 때(터치스크린이 있는 장치에선 탭 했을 때) 발생합니다.
- `contextmenu` – 요소 위에서 마우스 오른쪽 버튼을 눌렀을 때 발생합니다.
- `mouseover`와 `mouseout` – 마우스 커서를 요소 위로 움직였을 때, 커서가 요소 밖으로 움직였을 때 발생합니다.
- `mousedown`과 `mouseup` – 요소 위에서 마우스 왼쪽 버튼을 누르고 있을 때, 마우스 버튼을 뗄 때 발생합니다.
- `mousemove` – 마우스를 움직일 때 발생합니다.

### 2. 폼 요소 이벤트

- `submit` – 사용자가 `<form>`을 제출할 때 발생합니다.
- `focus` – 사용자가 `<input>`과 같은 요소에 포커스 할 때 발생합니다.

### 3. 키보드 이벤트

- `keydown`과 `keyup` – 사용자가 키보드 버튼을 누르거나 뗄 때 발생합니다.

### 4. 문서 이벤트

- `DOMContentLoaded` – HTML이 전부 로드 및 처리되어 DOM 생성이 완료되었을 때 발생합니다.

### 5. CSS 이벤트

- `transitionend` – CSS 애니메이션이 종료되었을 때 발생합니다.

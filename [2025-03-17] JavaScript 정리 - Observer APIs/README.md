# JavaScript 정리 - 6. Observer APIs

## 0️⃣ Observer APIs

`변경이 발생하면 콜백을 호출` 하는 순수 자바스크립트 API이다.

## 1️⃣ Mutation Observer

### 1. 정의

[mdn web docs - mutationObserver](<[https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)>)

DOM 트리의 형태나 구조의 변화를 감지한다.

### 2. 사용 예시

```jsx
// 1. 주기적으로 감지할 대상 요소 선정
const target = document.getElementById("id");

// 2. 옵저버 콜백 생성
const callback = (mutationList, observer) => {
  console.log(mutationList);
};

// 3. 옵저버 인스턴스 생성
const observer = new MutationObserver(callback); // 타겟에 변화가 일어나면 콜백함수를 실행하게 된다.

// 4. DOM의 어떤 부분을 감시할지를 옵션 설정
const config = {
  attributes: true, // 속성 변화 할때 감지
  childList: true, // 자식노드 추가/제거 감지
  characterData: true, // 데이터 변경전 내용 기록
};

// 5. 감지 시작
observer.observe(target, config);

// 6. 감지 중지
observer.disconnect();
```

### 3. 메서드

- `disconnect()`: `observe()`를 호출하기 전까지 `MutationObserver` 인스턴스가 더이상의 알림을 수신하지 않도록 설정
- `observe()`: 주어진 설정과 일치하는 DOM 변경이 발생했을 때 `MutationObserver` 인스턴스가 자신의 콜백으로 알림을 수신하도록 설정
- `takeRecords()`: 알림 큐를 비우고, 대기 알림을 배열로 반환

### 4. config 옵션

- `attributes`: 해당 노드의 attribute 속성이 변경될 때 변형 감지
- `childList`: 자식 노드에 발생하는 변경사항(추가 / 제거) 감지
- `subtree`: 감시 대상 요소와 모든 하위 노드들의 변화 감지
- `attributeFilter`:  추적을 원하는 attribute name들을 배열 형태로도 전달 가능(id, class)
- `characterData`: node.data에서 발생하는 변경사항 (text content)
- `attributeOldValue`: true이면 변경되기 이전과 이후의 attribute value를 콜백에서 모두 전달받을 수 있다. false인 경우 새로운 value만 콜백에 전달
- `characterDataOldValue`: true이면 변경되기 이전과 이후의 node.data를 콜백에서 모두 전달받을 수 있다. false인 경우 새로운 값만 콜백에 전달

<br>

## 2️⃣ IntersectionObserver

### 1. 정의

[mdn web docs - IntersectionObserver](<[https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API)>)

- 상위 요소 또는 html과 특정 대상 요소 사이의 변화 감지
- 대상 요소가 뷰포트 또는 루트 요소와 교차할 때 콜백 함수 호출
- 무한 스크롤, lazy loading에 사용한다.

### 2. 사용 예시

```jsx
let options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

// options에 따라 인스턴스 생성
let observer = new IntersectionObserver(callback, options);

// 타겟 요소 관찰 시작
let target = document.querySelector("#listItem");
observer.observe(target);
```

### 3. 메서드

- `observe(target)`: 타켓 요소에 대한 관찰 시작
- `unobserve(target)`: 타켓 요소에 대한 관찰 중지
- `disconnect()`: 인스턴스의 타켓 요소들에 대한 모든 관찰 중지

### 4. config 옵션

- `root`: 타겟 요소의 가시성을 확인할 때 사용되는 루트 요소
  - 설정하지 않거나 root값을 null로 주었을 때 기본값으로 **브라우저 뷰포트** 설정
- `rootMargin`: margin을 주어 루트 요소의 범위 확장
  - 확장된 영역 안에 타겟 요소가 들어가면 가시성에 변화 o
  - CSS의 margin과 유사하게 상/하/좌/우 각각 설정 가능
- `threshold`: 교차 판단 임계값(0~1 또는 배열)

<br>

## 3️⃣ ResizeObserver

### 1. 정의

[mdn web docs - ResizeObserver](<[https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver)>)

### 2. 사용 예시

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Resize Observer Example</title>
  <style>
    #resizable {
      width: 200px;
      height: 200px;
      background-color: lightblue;
      resize: both;
      overflow: auto;
    }
  </style>
</head>
<body>
  <div id="resizable">크기를 조절해보세요!</div>
  <script>
    // resizable 요소를 선택합니다.
    const resizableElement = document.getElementById('resizable');

    // ResizeObserver 인스턴스를 생성합니다.
    const resizeObserver = new ResizeObserver(entries => {
      // entries는 관찰 중인 요소들의 배열입니다.
      for (let entry of entries) {
        // 요소의 새로운 크기를 가져옵니다.
        const { width, height } = entry.contentRect;
        // 콘솔에 새로운 크기를 출력합니다.
        console.log(`새로운 크기: ${width}px x ${height}px`);
      }
    });

    // resizable 요소를 관찰 대상으로 설정합니다.
    resizeObserver.observe(resizableElement);
  </script>
</body>
</html></html>
```

### 3. 메서드

- `observe(target)`: 타켓 요소에 대한 관찰 시작
- `unobserve(target)`: 타켓 요소에 대한 관찰 중지
- `disconnect()`: 인스턴스의 타켓 요소들에 대한 모든 관찰 중지

### 4. config 옵션

특정 config 존재 x, 대신 콜백 함수의 값들을 기록한다.

- `target`: 관찰 대상 요소(element)
- `contentRect`: 관찰 대상의 사각형 정보(DOMRectReadOnly)
- `contentBoxSize`: 관찰 대상의 content-box(content) 크기
- `borderBoxSize`: 관찰 대상의 border-box(content + padding + border) 크기

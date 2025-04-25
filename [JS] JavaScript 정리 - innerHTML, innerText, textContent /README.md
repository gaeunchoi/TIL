# innerHTML, innerText, textContent 정리

---

`innerHTML`, `innerText`, `textContent` 속성은 텍스트를 읽어오고 설정할 수 있다는 점에서 유사하지만, 조금씩 다른 차이가 존재한다.

### 1️⃣ innerHTML

`innerHTML`은 요소(element) 내에 포함 된 **HTML 또는 XML 마크업**을 가져오거나 태그와 함께 입력하여 내용을 직접 설정한다.

```jsx
// html 코드와 함께 작성
document.element.innerHTML = "<p>innerHTML로 설정했다</p>";

// 빈 문자열을 넣으면 요소의 내용을 전부 지운다.
document.element.innerHTML = "";

// 스타일 적용도 가능하다
document.element.innerHTML =
  "<span style='color:pink'>innerHTML로 스타일도 설정했다</span>";
```

### 2️⃣ innerText

`innerText`는 요소(element)의 속성으로, 요소 내에서 사용자에게 보여지는 **text 값**들을 가져오거나 설정할 수 있다.

```jsx
// innerHTML과 달리 text값만 다룬다.
document.element.innerText = "innerText로 설정했다";

// html 태그를 넣어도 싹 다 text값으로 인식한다.
document.element.innerText = "<p>innerText로 태그까지 넣는다</p>";
```

### 3️⃣ textContent

`textContent`는 노드(Node)의 속성으로, `innerText`와는 달리 `<script>`나 `style` 태그에 상관없이 해당 노드가 가지고 있는 **text 값**을 모두 읽어온다.

```jsx
<div id="divv">
  Hi!
  <span style="display: none">InnerText는 못본다</span>
</div>
```

```jsx
const content = document.getElementById("divv");
console.log(content.textContent);
// 안녕
// innerText는 못본다
```

### 통합 예제 코드

```jsx
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>

  <body>
    <div id="content">
      HELLO GUYS!
      <span style="display: none">innerText만 나를 볼 수 없음!</span>
    </div>

    <script>
      const content = document.getElementById("content");

      console.log(content.innerHTML);
      // html 전체를 다 가져옴
      // HELLO GUYS!
      // <span style="display: none">innerText만 나를 볼 수 없음!</span>

      console.log(content.innerText);
      // 사용자에게 보여지는 텍스트만 가져옴
      // 숨겨진 텍스트는 사용자에게 보여지지 않기 때문에 HELLO GUYS!만 가져옴
      // HELLO GUYS!

      console.log(content.textContent);
      // 숨겨진 텍스트까지 포함해서 텍스트값을 모두 다 가져옴
      // HELLO GUYS!
      // innerText만 나를 볼 수 없음!
    </script>
  </body>
</html>

```

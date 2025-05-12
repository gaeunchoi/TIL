React로 API 통신을 할 때 처음엔 브라우저 내장 함수인 `fetch()`를 많이 사용했지만,

- 매 번 JSON 파싱을 해야함,
- HTTP 에러 throw가 안됨
- 요청/응답 가로채기(interceptor) 기능 없음
- 기본 timeout 설정 없음

등의 이유로 더 다양한 기능을 제공하는 `Axios`를 사용하게 되었다.

# 1. Axios 설치 후 import

```bash
npm install axios
또는
yarn add axios
```

```bash
import axios from "axios"
```

# 2. Axios 기본 사용법

## 📤 GET 요청

`GET` 요청은 서버에서 어떠한 데이터를 가져오는 것을 의미한다.

```jsx
// 그냥 막 지어낸 ,,, 코드다 ,,,
import axios from "axios";

axios
  .get("와랄라URL와랄라")
  .then((res) => console.log(res.data))
  .catch((err) => console.error(err));
```

## 📥 POST 요청

`POST` 요청은 새로운 리소스를 생성할 때 사용한다.

예시에서 `Content-Type`은 요청 본문 형식을 명시, `Authorization`은 토큰을 헤더에 실어 인증처리하는 것을 의미한다.

```jsx
// 그냥 막 지어낸 ,,, 코드다 ,,,
import axios from "axios";

axios
  .post(
    "와랄라URL와랄라",
    {
      name: "람쥐",
      age: 26,
    },
    {
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${token}`,
      },
    }
  )
  .then((res) => console.log(res.data))
  .catch((err) => console.error(err));
```

# 3. Axios 인스턴스 생성 ⭐️

카카오톡 클론코딩 리팩토링을 하면서 기존에 fetch로 API를 불러왔는데 Axios로 바꾸는 작업을 진행했다.

그 때 Axios 인스턴스를 생성해서 리팩토링을 하니까 코드가 아주 깔끔해졌지 뭡니까 ? 암튼 렛츠고

React 프로젝트에서 API 호출을 할 때 보통 다음과 같은 설정들이 반복된다.

- 공통 `baseURL` (API 서버 주소)
- 공통 헤더(ex: Authorization 토큰)
- 공통 응답/에러처리

이걸 매번 fetch에 붙이니 난리가 났고 ,,, 그냥 axios를 써도 마찬가지다. 중복 코드가 미친듯이 불어나고 관리가 안되었다.. 그러면 어떻게 해결할 수 있는가 ?

> 💡 Axios 인스턴스(`axios.create`)로 공통 설정을 묶어서 재사용하자 !

```jsx
// api.js
import axios from "axios";

const axiosInstance = axios.create({
  baseURL: "https://example.com",
  timeout: 1000,
  headers: {
    "Content-Type": "application/json",
  },
});

export default axiosInstance;
```

```jsx
// example.js
import axiosInstance from "./api";

const example = async () => {
  const res = await axiosInstance({
    method: "GET",
    url: `/{endpoint 이후}`,
  });
  console.log(res.data);
};
```

# 4. Axios Interceptor 인터셉터 ⭐️

인터셉터는 다음과 같은 상황에 사용된다.

- 매 요청마다 토큰을 자동으로 첨부하고 싶을 때,
- 요청 / 응답을 로그로 출력하거나 수정할 때

## 📤 요청(Request) 인터셉터

`요청 인터셉터`는 서버로 요청을 보내기 전에 실행이 된다.

이는 **토큰 삽입**, **커스텀 헤더 추가**, **로그 출력** 등이 가능하다.

```jsx
// api.js
axiosInstance.interceptors.**request**.use(
  config => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  error => Promise.reject(error)
);
```

## 📥 응답(Response) 인터셉터

`응답 인터셉터`는 서버에서 응답을 받은 직후에 실행이 된다.

이는 **특정 에러 코드에 따른 처리**, **응답 데이터 가공** 등이 가능하다.

```jsx
// api.js
axiosInstance.interceptors.**response**.use(
  response => response,
  error => {
    const { status } = error.response || {};
    if (status === 401) {
      alert('로그인이 만료되었습니다');
    }
    return Promise.reject(error);
  }
);
```

# 5. 에러 처리

Axios는 HTTP 상태 코드가 에러일 경우 자동으로 `catch` 블록으로 넘어간다.

이 때, Axios의 에러 객체는 `e.response`, `e.request`, `e.message`로 구성되어있다.

- `e.response`: 서버에서 응답이 왔으나 오류인 경우(ex: 404, 500)
- `e.request`: 요청은 되었으나 응답이 없을 경우(네트워크 오류 등)
- `e.message`: 기타 오류 메시지

```jsx
console.log(e.response);
/*
{
	status: 404,
	data: { message: "Not Found" },
	header: { ... },
	config: { ... }
}
*/
```

```jsx
try {
  await axiosInstance({
    method: "GET",
    url: "/404-page",
  });
} catch (e) {
  if (e.response) {
    console.log(e.response.status);
  } else {
    console.log(e.message);
  }
}
```

# 6. fetch vs. axios 정리

## 기본 개념

| 항목      | Fetch                             | Axios                                     |
| --------- | --------------------------------- | ----------------------------------------- |
| 정의      | 브라우저 내장 HTTP API            | Promise 기반의 HTTP 클라이언트 라이브러리 |
| 설치 여부 | ❌ (기본 제공)                    | ✅ (`npm install axios`)                  |
| 지원 환경 | 브라우저, Node.js (18+ 일부 내장) | 브라우저, Node.js 모두 지원               |

## 기능 비교

| 항목               | Fetch                                            | Axios                                        |
| ------------------ | ------------------------------------------------ | -------------------------------------------- |
| 기본 사용법        | `fetch(url)`                                     | `axios.get(url)` 등 메서드 별 API            |
| 응답 처리          | `response.json()` 등 수동 파싱 필요              | 자동으로 JSON 파싱 (`response.data`)         |
| 요청/응답 인터셉터 | ❌ 직접 구현 필요                                | ✅ `axios.interceptors`로 지원               |
| 요청 취소          | ❌ `AbortController`로 복잡하게 구현             | ✅ `CancelToken` 또는 `AbortController` 사용 |
| HTTP 오류 처리     | ❌ 네트워크 오류만 reject, HTTP 오류는 수동 처리 | ✅ HTTP 오류도 자동으로 catch 가능           |
| 요청 타임아웃      | ❌ 직접 설정 필요                                | ✅ `timeout` 옵션 지원                       |
| 브라우저 지원      | 최신 브라우저만 (IE 미지원)                      | 더 넓은 호환성 (polyfill 가능)               |

## 코드 예시 비교

### Fetch 사용 예시

```jsx
fetch("https://example.com")
  .then((response) => {
    if (!response.ok) throw new Error("HTTP Error");
    return response.json();
  })
  .then((data) => console.log(data))
  .catch((error) => console.error("Fetch Error:", error));
```

### Axios 사용 예시

```jsx
import axios from "axios";

axios
  .get("<https://api.example.com/data>")
  .then((response) => console.log(response.data))
  .catch((error) => console.error("Axios Error:", error));
```

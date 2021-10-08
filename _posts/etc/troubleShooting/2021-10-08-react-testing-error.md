---
title: "[Trouble Shooting] react에서 Plotly.js 테스팅시 다양한 error"
excerpt: ""

category: TroubleShooting
tags:
  - [React, plotly, react-testing-library, jest, error]

toc: true
toc_sticky: true

date: 2021-10-08
last_modified_at: 2021-10-08
---

## 1. 'TypeError: window.URL.createObjectURL is not a function'

![img](https://blog.kakaocdn.net/dn/mCDtS/btq7qugMS1u/gkMGMwhkZV001j1hArUnJK/img.png)

### URL.createObjectURL()

이 메소드를 알기 전에 먼저 **Blob 객체**부터 알아야 한다.

**Blob**

- Blob은 이미지, 사운드, 비디오와 같은 멀티미디어 데이터를 다룰 때 사용한다.
- Blob 객체를 통해 데이터의 크기 및 MIME 타입을 알아내는 등의 작업을 할 수 있다.
- **Blob 객체를 url로 만들 때** URL. createObjectURL 메소드를 사용한다.
- URL. createObjectURL()메소드는 Blob 객체를 나타내는 URL을 포한한 다음과 같은 DOMString을 생성한다.
- **(blob: URL)**

**문제 발생**

jest로 react testing 중 Plotly.js 차트를 사용한 컴포넌트가 렌더링 됐을 때 발생

**해결 과정**

jest 에서 URL.createObjectURL() 메소드를 지원하지 않는다는 것 같다.<br>
setupTests.ts에서 메소드를 전역적으로 mock 처리 해주었다.

**해결**

```react
window.URL.createObjectURL = jest.fn();
```

Plotly.js를 import한 컴포넌트가 렌더링 되었을 때 window.URL.createObjectURL를 mock 처리 한 것으로 해결 되었다.

---

## 2. 'Error: Not implemented: HTMLCanvasElement.prototype.getContext (without installing the canvas npm package)'

**문제 발생**

jest로 react testing 중 Plotly.js 차트를 사용한 컴포넌트가 렌더링 됐을 때 발생. <br>
consle.error이다.

**해결 과정**

JSDom 은 getContent메소드를 구현하지 않고,<br>
'Error: Not implemented: HTMLCanvasElement.prototype.getContext' 에러를 던진다.<br>
위의 메소드를 setupTests.ts에서 전역적으로 mock 처리 해주었다.

**해결**

```react
window.HTMLCanvasElement.prototype.getContext = jest.fn();
```

---

## 3. 'Error: Uncaught [TypeError: window.ResizeObserver is not a constructor]'

**문제 발생**

위의 에러들을 차례대로 해결해주고 나니까 마지막으로 이 에러가 출력 됐다.

**해결 과정**

에러 그대로 **window.ResizeObserver** **생성자**를 mock함수로 만들고 내부 메소드들을 하나씩 mocking 했다.

**해결**

```react
beforeEach(() => {
  window.ResizeObserver = jest.fn().mockImplementation(() => ({
    observe: jest.fn(),
    unobserve: jest.fn(),
    disconnect: jest.fn()
  }));
});

afterEach(() => {
  window.ResizeObserver = ResizeObserver;
  jest.restoreAllMocks();
});
```

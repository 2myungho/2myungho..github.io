---
title: "[Trouble Shooting] Javascript에는 있지만 Typescript에는 없는 속성일 경우"
excerpt: "라이브러리를 사용하다보면 typescript가 적용 안 된 속성이 간혹 나온다."

category: TroubleShooting
tags:
  - [React, plotly, typescript]

toc: true
toc_sticky: true

date: 2021-10-12
last_modified_at: 2021-10-12
---



### 문제발생

react typescript로 Plotly.js 차트 라이브러리를 사용하는데 문제가 발생.

히스토그램 차트의 속성 중에 **xbins** 와 **ybins**가 있다.
수직 히스토그램에서 xbins 속성은 잘 사용 되지만, 수평 히스토그램에서 ybins 속성은 에러가 나며 사용이 되지 않았다.

 

### 해결 과정

공식 홈페이지 차트 에디터에서는 ybins속성이 적용되는 걸 확인했다,
타입스크립트 문제라 생각하고 data 속성의 Data[] 타입을 타고 들어가서 xbins와 ybins를 검색해봤다.

![img](https://blog.kakaocdn.net/dn/cjd172/btq7p9p6YDQ/AniA6Kr4VoW4GXvjyKuJK1/img.png)

그 결과 xbins 속성은 있었지만 **ybins속성은 없었다..**

 

### 해결

타입스크립트의 타입 단언으로 해결할 수 있었다.

```typescript
//타입 단언 예제
type TestType = { a: string };
const test = { a: '', b: 0 } as TestType

console.log(test) //{a: "", b: 0}
console.log(test.b) // error
```

위에 예제처럼 사용해서 **TestType**에 있는 변수만 검사하고, 컴파일을 통과할 수 있다.
(즉, 타입 검사를 할 때 TestType에 있는 변수만 최소한으로 검사하는 방법이다. )

위와 같은 방법을 프로젝트에 적용했다.

```react
<Plot
  data={[
    {
      type: 'histogram',
      y,
      ybins: {
        start: 'LOT_1',
        end: 'LOT_2',
        size: 0
      }
    } as Data
  ]}
</>
```

**ybins** 는 Data 타입 검사에서 제외되지만 javascript의 속성으로 사용이 될 수 있다.
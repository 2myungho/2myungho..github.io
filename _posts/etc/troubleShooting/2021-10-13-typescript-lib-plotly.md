---
title: "[Trouble Shooting] Typescript 상속을 이용해 라이브러리에 없는 타입 지정"
excerpt: "(React+ Typescript + Plotly js)"

category: TroubleShooting
tags:
  - [React, plotly, typescript]

toc: true
toc_sticky: true

date: 2021-10-13
last_modified_at: 2021-10-13
---

### 타입 캐스팅

```typescript
interface A {
  key: number;
  value: number;
  test1: number;
}

interface AA {
  key: number;
  value: number;
  test2: number;
}

const a: AA = {
  key: 1,
  value: 2,
  test2: 3,
};
interface B extends AA {
  test1: number;
}
interface C {
  test3: number;
}

console.log((a as unknown as A).test1); // undifind (AA와 A는 연관된 관계가 아니다.)
console.log((a as B).test1); // undifind (AA와 B는 연관된 관계이다.)
console.log((a as B).test2); // 3
console.log(a as unknown as C); // {key: 1, value: 2, test2: 3}

/*
  자식-부모관계(연관된 관계)가 아니라면 as unknown을 먼저 정의해 주어야 한다. 
  */

/* 
  타입 캐스팅을 해줌으로써 사용하는 요소가 정의되지 않았더라도 타입을 정의해 줄 수 있다.
  라이브러리를 사용하는데 사용하려는 속성이 type 정의가 안되어 있다면 
  타입 캐스팅을 통해 정의해 줄 수 있다. 
  
  쉽게 말해서 타입 캐스팅을 해주면 타입 내에 있는 요소만 검사하고,
  없는 요소는 검사하지 않고 지나간다고 이해했다.
  */
```

### 문제 발생

![img](https://blog.kakaocdn.net/dn/pXLEp/btq7sFbbcvV/ZptIsINL67iKkrTdrXOvY1/img.png)

### 원인

![img](https://blog.kakaocdn.net/dn/53fZp/btq7uTMPXB1/0q6ekepwxx5lZFyqRcLmLK/img.png)

내가 필요한 부분은 **point[0].label**이었다.

**points**의 타입을 확인하면

![img](https://blog.kakaocdn.net/dn/bqAvDf/btq7ttut8aV/3FoG3kiF5DcNEVxK24EaT1/img.png)

**PlotDatum** 타입이 내가 사용하려는 **point[0].label**의 타입이다.<br>
이런.. 확인해보니 내가 필요한 label 부분만 쏙 빠져 있었다!<br>
(plotly를 사용하다 보면 **history의 ybins, point의 label**등 빠져 있는 변수들이 간혹 보인다. )

### 문제 해결

```react
 interface PieEvent extends PlotMouseEvent {
    points: PieDatum[];
  }
  const onGetImpLabel = (e: PieEvent) => {
    console.log(e);
    dispatch(setImpLabel(e.points[0].label));
  };
```

페이지 맨 위의 예제와 비슷한 방법으로 해결했다.<br>
label이 있어야 할 **PlotDatum** 타입을 상속 받아 자식 타입에 label 을 작성 해준다.

```react
interface PieDatum extends PlotDatum {
    label: string;
  }
```

마찬가지로 **PlotMouseEvent** 의 타입을 상속 받아 자식 타입인 **PieEvent**을 생성하고,
**PlotDatum**의 자식 타입인 **PieDatum**타입을 **points**의 타입으로 정의해 준다

```react
interface PieEvent extends PlotMouseEvent {
    points: PieDatum[];
  }
```

이제 event 파라미터에 **PieEvent** 타입을 정의해주면 된다.

```react
const onGetImpLabel = (e: PieEvent) => {
    dispatch(setImpLabel(e.points[0].label));
  };
```

### 자기 타입을 다시 타입 캐스팅 해줘야 하는 이유는?

위와 같이 했는데 onClick 이벤트에서 에러가 났다

![img](https://blog.kakaocdn.net/dn/bwAtVN/btq7utHxGj8/7ivzzK4OZhjipAcQhK2UC0/img.png)

**onGetImpLabel함수**에서 onClick event 파라미터의 타입과 다른 타입을 사용한 탓인 것 같다.<br>
그래서 onClick이벤트의 기존 타입을 onGetImpLabel함수에 **타입 캐스팅**을 해준다.

```react
onClick={onGetImpLabel as (event: Readonly<PlotMouseEvent>) => void}
//자식-부모관계(연관된 관계)이므로 unknown을 생략할 수 있다.
```

### **[plotly.js-react](https://github.com/plotly/react-plotly.js/issues/247)** 깃허브 이슈에 등록

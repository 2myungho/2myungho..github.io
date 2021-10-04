---
title: "Object.keys 메소드 객체 순회"
excerpt: "Object 메소드 사용시 순서 보장"

category: Js
tags:
  - [Object, loop]

toc: true
toc_sticky: true

date: 2021-10-04
last_modified_at: 2021-10-04
---

회사에서 프로젝트를 진행하는데 데이터의 값이 원하는 순서대로 들어오지 않았다. 어디서 문제가 났는지 디버그를 해보니 `Object.value`메소드에서 객체의 순서를 key 값의 오름차순으로 자동 변환 시켰다.

#### 객체

- 객체는 배열과 다르게 index(순서)가 없다. 그렇기 때문에 어떤 순서로 객체의 키 값에 접근하는지 알 수 없다.
- 객체의 값은 키를 통해서만 접근이 가능하다.
- 순서가 보장되지 않은 순회라고 한다.

##### Object.keys()

- [mozilla 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)
- 메소드는 주어진 객체의 속성 이름들을 일반적인 반복문과 동일한 순서로 순회되는 열거할 수 있는 **배열로 반환**합니다

아래의 예시 코드를 보자

```javascript
const test = {
  1: { a: 1, b: 1 },
  4: { a: 4, b: 4 },
  2: { a: 2, b: 2 },
  3: { a: 3, b: 3 },
};
console.log(test);
// 1:Object, 4:Object, 2:Object, 3: Object

const testValue = Object.keys(test);
console.log(testValue);
// ["1", "2", "3", "4"]
```

객체의 키를 작성할 때 1,4,2,3 순으로 작성했지만 Object.keys()메소드에서 배열로 반환 될 때 순서가 보장되지 않았다.

key 값이 알파벳이라면 작성한 순서대로 출력이 되었지만 숫자일 때는 오름차순 순서로 반환 돼서 출력된다. 아직 해당하는 문제에 대한 이유는 찾지 못했다.

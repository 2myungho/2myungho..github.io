---
title: "[Trouble Shooting] React Testing에서 AntD 라이브러리의 Dropdown 검사"
excerpt: "힌트는 비동기"

category: TroubleShooting
tags:
  - [React, ReactTesting, antD, dorpdown]

toc: true
toc_sticky: true

date: 2021-10-07
last_modified_at: 2021-10-07
---

### 문제 발생

React Testing 중 아래의 코드에서 계속 문제가 생겼다.

```react
test('test', () => {
  //given
  render(<Header />);
  const menuButton = screen.getByText('사내문서');
  //when
  fireEvent.mouseOver(menuButton);
  const dropdownMenu = screen.getByText('도서대장');
  //then
  expect(dropdownMenu).toBeDefined();
});
```

먼저 ant Design 의 드롭다운을 사용했다.

드롭다운 시 도서대장 메뉴가 DOM에 출력 되는지 확인하기 위한 테스팅 코드이다. 문제가 없어 보이는 코드이지만 screen.debug() 실행시 '도서대장' Text가 출력이 되지 않는다.

즉, mouseOver 이벤트가 작동이 안 된다. 왜 그럴까?

### 해결 과정

- 먼저 도서대장 메뉴가 드롭다운(hover)이 아닌 클릭 이벤트 시 작동 하도록 코드를 수정했다. 결과는 테스트 성공이었다.

- 그럼 click 이벤트와 hover 이벤트 발생 시 서로 차이가 뭘까 생각을 해봤다.

- 앱 화면에서 clilck 시에는 바로 ‘도서대장’ 메뉴가 나타나지만 dropdown시 약간의 딜레이가 발생하는 걸 인지하게 됐다.

- 만약 antD 드롭다운에 타임아웃 함수가 사용 됐다면 테스트 코드에 async await을 사용해서 ‘도서대장’ 메뉴가 나타날 때까지 기다려 보는 코드를 작성했다.

  ```react
  test('test', async () => {
    //given
    render(<Header />);
    const menuButton = screen.getByText('사내문서');
    //when
    fireEvent.mouseOver(menuButton);
    await waitFor(() => screen.getByText('도서대장'));
    //then
    const dropDownMenu = screen.getByText('도서대장');
    expect(dropDownMenu).toBeDefined();
  });
  ```

### 해결

async await을 사용해서 코드를 수정하니 테스트가 성공했다.

andD 드롭다운에 trigger 옵션을 click으로 주면 딜레이가 발생하지 않지만 hover이면 딜레이가 발생하도록 만든 것 같다. ‘도서대장’ 메뉴가 생성되기 전에 debug와 expect가 실행되어 출력되지도 않고 계속 테스트가 실패했었다.

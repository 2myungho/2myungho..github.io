---
title: "[Trouble Shooting] 라우터 컴포넌트가 아닌 컴포넌트의 주소 값 받기"
excerpt: "고차함수 withRouter"

category: TroubleShooting
tags:
  - [React, router, withRouter]

toc: true
toc_sticky: true

date: 2021-10-05
last_modified_at: 2021-10-05
---



### 문제 발생

페이지가 이동하면 **window.location.pathname**의 값이 페이지 별로 받아와야 하는데 로딩 시에만 받아와서 헤더 컴포넌트가 원하는 화면에 출력되지 않음



### 해결 과정

contents 컴포넌트는 router가 적용된 컴포넌트가 아니기 때문에 window.location.pathname의 값이 로딩 될 때 한 번 받아오고 컴포넌트 끼리 라우터 될 때는 값을 받아오지 못했다고 생각함.



### 해결

**withRouter**함수로 라우터 컴포넌트가 아닌 contents컴포넌트를 감싸면 해당 컴포넌트에 라우터 기능이 적용 되어 history, match 등의 함수를 사용할 수 있으며, 페이지 이동 시(라우터 페이지) 주소 값을 받아올 수 있음

```react
function Contents({ history }: RouteComponentProps): JSX.Element {
  return (
    <>
      {history.location.pathname !== '/' && <Header />}
      <Route exact path="/main" component={Main} />
    </>
  );
}
export default withRouter(Contents);
```

이렇게 컴포넌트를 취하여 새로운 컴포넌트를 반환하는 함수를 **고차 컴포넌트**라고 한다.

 

고차컴포넌트 [공식문서](https://reactjs-kr.firebaseapp.com/docs/higher-order-components.html)
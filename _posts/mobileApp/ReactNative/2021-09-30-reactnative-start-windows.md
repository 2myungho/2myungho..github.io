---
title: "Windows에서 React native 환경 구축부터 실행까지"
excerpt: "React native를 시작해보자!"

category: ReactNative
tags:
  - [mobile, react, react-native]

toc: true
toc_sticky: true

date: 2021-09-30
last_modified_at: 2021-09-30
---

#### 참조

- 윈도우에서 react native 개발환경 구축 블로그
  - [https://dev-yakuza.posstree.com/ko/react-native/install-on-windows/](https://dev-yakuza.posstree.com/ko/react-native/install-on-windows/)

위 글에서 추가 내용으로 아래와 같은 에러가 발생했을 때

```shell
error Failed to install the app. Please accept all necessary Android SDK licenses using Android SDK Manager: "$ANDROID_HOME/tools/bin/sdkmanager --licenses".
Error: Command failed: gradlew.bat app:installDebug -PreactNativeDevServerPort=8081

FAILURE: Build failed with an exception.
```

안드로이드 스튜디오에서 **Android SDK Command-line Tools** 를 설치해 줌으로 해결이 되었다.

![img](https://blog.kakaocdn.net/dn/bhIZGL/btrfREbbDp0/8QPnjKfst1Bza7SWM4ftfk/img.png)

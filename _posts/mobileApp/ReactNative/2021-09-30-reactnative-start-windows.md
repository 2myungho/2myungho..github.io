---
title: "Windows에서 React native 환경 구축부터 실행까지"
excerpt: "React native를 시작해보자!"

category: ReactNative
tags:
  - [mobile, react, react-native, android, windows]

toc: true
toc_sticky: true

date: 2021-09-30
last_modified_at: 2021-10-01
---

#### 순서

##### 0. Chocolatey 설치 (선택)

##### 1. Nodejs 설치

##### 2. Python 설치

- RN의 빌드 시스템은 파이썬을 사용한다.

##### 3. JDK 설치

- 안드로이드로 개발하기 위해 JDK가 필요하다.
- react-native-cli로 실행시 Windows는 안드로이드만 확인할 수 있다.

##### 4. 안드로이드 스튜디오 설치

- 설치창에서 계속 다음 단계로 이동해주면 된다.

##### 5. SDK Manager

![RN_start2](https://user-images.githubusercontent.com/52882578/135443951-116e0edb-492f-4767-a387-2fd401873cfe.PNG)

- 하단에 Show Pacakge Details를 체크해주고 아래 항목을 모두 체크해준 뒤 하단의 OK버튼을 클릭하고 설치해준다.
- **Android SDK Platform 29**
- **Intel x86 Atom System Image**
- **Google APIs Intel x86 Atom System Image**
- **Google APIs Intel x86 Atom_64 System Image**

##### 6. 안드로이드 스튜디오 환경 변수 설정

- 새 환경 변수 추가에서 변수 이름은 `ANDROID_HOME`을 입력하고 변수 값에는 `안드로이드 스튜디오 SDK` 위치를 넣어준다.

- `path`변수에는 안드로이드 스튜디오 SDK 하위 폴더에 있는 `platform-tools`폴더를 설정해 주어야 한다.

- 자신의 안드로이드 스튜디오 SDK의 위치는 SDK Manager 상단에서 확인할 수 있다.

  ![RN_start4](https://user-images.githubusercontent.com/52882578/135447476-cfd47aef-092f-447b-bc8e-579475e5846f.PNG)

- `C:\Users\AppData\Local\Android\Sdk\platform-tools` 와 같은 형식으로 path에 설정해준 뒤

- 명령 프롬프트(cmd)에서 `adb`명령어를 입력해주면 안드로이드 스튜디오의 정보를 확인할 수 있다.

##### 7. AVD Manager

![RN_start3](https://user-images.githubusercontent.com/52882578/135443953-a2bce7eb-72bd-48de-854f-05843d413617.PNG)

- AVD 매니저에서 가상 기기를 만들어 실행한다.
- 가상 기기가 실행되어 있지 않으면 프로젝트가 실행되지 않는다.

##### 8. React native CLI 설치 및 프로젝트 생성

- 본인은 expo가 아닌 [React native CLI](https://reactnative.dev/docs/environment-setup)로 시작했다.

- [공식문서](https://reactnative.dev/docs/environment-setup)의 **Creating a new application** 부분을 확인하면 아래와 같은 내용을 확인할 수 있다.

  > React Native has a built-in command line interface, which you can use to generate a new project. You can access it without installing anything globally using `npx`, which ships with Node.js.
  >
  > React Native에는 새 프로젝트를 생성하는 데 사용할 수 있는 내장 명령줄 인터페이스가 있습니다. `npx`Node.js와 함께 제공되는 를 사용하여 전역적으로 아무것도 설치하지 않고도 액세스할 수 있습니다
  >
  > 글로벌로 `react-native-cli` 설치했던 적이 있을 경우 문제가 발생할 수 있기 때문에 설치한 적이 있다면 `npm uninstall -g react-native-cli` 명령어로 삭제해준다.

- RN 프로젝트 생성

  ```shell
  npx react-native init projectName
  ```

- RN Typescript 프로젝트 생성

  ```shell
  npx react-native init MyApp --template react-native-template-typescript
  ```

  정상적으로 생성 됐다면 아래와 같은 화면을 볼 수 있다.

  ![RN_start](https://user-images.githubusercontent.com/52882578/135449273-371d20a5-1d4d-4181-a3a9-1940957f985c.PNG)

##### 8. 프로젝트 실행

```shell
npm run android
# or
yarn android
```

위 글에서 추가 내용으로 아래와 같은 에러가 발생했을 때

```shell
error Failed to install the app. Please accept all necessary Android SDK licenses using Android SDK Manager: "$ANDROID_HOME/tools/bin/sdkmanager --licenses".
Error: Command failed: gradlew.bat app:installDebug -PreactNativeDevServerPort=8081

FAILURE: Build failed with an exception.
```

안드로이드 스튜디오에서 **Android SDK Command-line Tools** 를 설치해 줌으로 해결이 되었다.

![img](https://blog.kakaocdn.net/dn/bhIZGL/btrfREbbDp0/8QPnjKfst1Bza7SWM4ftfk/img.png)

#### 참조

- 윈도우에서 react native 개발환경 구축 블로그
  - [https://dev-yakuza.posstree.com/ko/react-native/install-on-windows/](https://dev-yakuza.posstree.com/ko/react-native/install-on-windows/)

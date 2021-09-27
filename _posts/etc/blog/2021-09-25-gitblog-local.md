---
title: "[Github 블로그](Windows) github blog local에서 실행하기"
excerpt: "원격 저장소에서 확인하지 말고 로컬에서 먼저 확인하자."

category: Blog
tags:
  - [Blog, jekyll, Ruby, Github]

toc: true
toc_sticky: true

date: 2021-09-25
last_modified_at: 2021-09-27
---

# 1. Github에서 Repository 생성

Repository를 생성할 때 자신의 깃허브 계정 이름으로 생성해야 한다. <br/>
ex) `2myungho.github.io`

![blog create](https://user-images.githubusercontent.com/52882578/134810917-b711f295-7ac6-4baa-8e38-0b343f2f718b.PNG)

Repository 생성 후 <br/>
![blog create2](https://user-images.githubusercontent.com/52882578/134810997-f2d0051a-ae45-454c-9707-1eef97a6fd52.PNG)

Import code를 클릭하고 원하는 `jekyll 테마`의 URL을 넣어주고 Begin import 버튼을 클릭한다.

> [jekyll 테마](http://jekyllthemes.org/) <br/>
> 본인은 [minimal mistakes](https://github.com/mmistakes/minimal-mistakes) 테마를 사용했다. 사용한 이유는 사람들이 많이 사용하는 것 같고, 심플한 디자인이 좋았기 때문이다.

![blog create3](https://user-images.githubusercontent.com/52882578/134811190-694a3739-bb11-446b-8851-94fef2edcb63.PNG)

![blog create4](https://user-images.githubusercontent.com/52882578/134811296-bc9fe45c-f904-42f3-a3e6-265c97b44a42.PNG) <br/>
`Begin import`를 클릭하면 몇 분의 시간이 소요된 후 [minimal mistakes](https://github.com/mmistakes/minimal-mistakes) 테마가 적용된 Repository가 생성된다. <br/>
`자신의 계정.github.io` 으로 생성된 레포지토리는 `자신의 계정.github.io` 을 주소창에 입력하면 자동으로 호스팅이 되는 것을 확인할 수 있다.

# 2. 로컬에서 Blog 확인하기

## 1) git clone

생성한 Repository를 `git clone` 명령어로 로컬에 받아온다.

## 2) Ruby 설치

`Jekyll`은 Runy 언어로 만들어졌기 때문에 `Jekyll`을 설치하기 전에 [Ruby](https://rubyinstaller.org/downloads/)를 먼저 설치해야 한다.
Ruby를 설치할 때는 Window용으로 꼭! `WITH DEVKIT`을 설치해야 한다. ([Jekyll windows 설치](https://jekyllrb.com/docs/installation/windows/))

![blog create5](https://user-images.githubusercontent.com/52882578/134813852-3261d9a7-1dd2-4280-b03d-a7d7b089a0b7.PNG)

설치 중에 `Add Ruby executables to your PATH` 옵션을 체크하면 윈도우에서 환경 변수를 설정하는 번거로움을 생략할 수 있다.

설치가 완료 되면 Ruby cmd 창이 실행되는데 만약 창을 닫게 되면 Ruby cmd에서 `ridk install` 명령어로 실행할 수 있다. 아래와 같은 화면이 나타나면 ENTER를 클릭해 `MSYS2`를 설치해준다.

![blog create6](https://user-images.githubusercontent.com/52882578/134813947-b1e8a9b5-5604-44de-b653-47dec428ed62.PNG)

## 3) Jekyll과 Bundler 설치

[Jekyll 공식홈페이지](https://jekyllrb-ko.github.io/)

루비 설치가 완료되면 Ruby cmd에서 `gem install bundler jekyll` 명령어로 Jekyll과 Bundler를 설치한다.
설치가 완료되면 `jekyll -v` 명령어로 Jekyll이 제대로 설치 되었는지 확인한다.

## 4) bundle install

Jekyll까지 설치 되었으면 블로그를 clone한 디렉터리로 이동한다.
최상위 파일 중에 `Gemfile`이 생성된 것을 확인할 수 있는데 이 Gemfile을 수정하고 필요한 플로그인을 설치해야 실행이 되었다. 본인은 이 중 한 개라도 설치가 안 되어 있으면 error가 발생했다. (windows기준, 21-09-27)

![blog create7](https://user-images.githubusercontent.com/52882578/134814495-79156b2f-515f-41d6-9cac-942bdc95e209.PNG)

1. **gem 'github-pages'**
   - https://github.com/github/pages-gem
2. **gem 'jekyll-include-cache'**
   - https://github.com/mmistakes/minimal-mistakes/issues/1937
3. **gem "webrick", "~> 1.7"**
   - https://junho85.pe.kr/1850
   - ruby 3.0.0부터 webrick이 기본으로 포함된 gem에서 빠졌기 때문에 설치해야 하는 것 같다.

위와 같이 작성해준 뒤 터미널에서 `bundle install` 명령어를 입력한다.

## 5) 실행

실행하기에 앞서 `_config.yml`파일의 url 부분을 `url: http://localhost:4000`로 변경해준다.

실행은 터미널에 `bundle exec jekyll serve` 명령어를 입력해주면 `localhost:4000`으로 접속할 수 있게 된다

---
title: '프론트엔드 개발자가 되기 위해 공부해야 할 것들'
date: 2020-09-15 00:27:00
category: 'etc'
draft: false
---

회사에서 주로 `React Native` 개발을 해왔고 가끔씩 인앱 브라우저에서 사용하기 위한 웹개발을 위해 또는 간단한 소개페이지 제작을 위해 웹페이지를 만들어야 했다. 그리고 웹공부를 열심히 해야겠다고 생각했다

알면 알수록 어려운 분야이고 공부해야할게 많은 분야이다. 좋은 프론트엔드 개발자가 되기 위해 나름대로 공부해야 할 내용을 정리해보려고 한다

## html / css

우리가 보는 브라우저의 모든 화면은 html로 표시되며 css는 문서형식의 기본골격을 가지고 있는 html을 꾸며준다

   <img src="https://media.giphy.com/media/fuJPZBIIqzbt1kAYVc/giphy.gif" width="100%" height="auto" />

> via GIPHY

우리나라는 특이하게 퍼블리셔라는 직군을 따로 갖고 있고 대부분의 큰 기업은 퍼블리싱팀을 따로 갖고 있다. 하지만 대부분의 프론트엔드 개발자는 html / css에 대한 지식을 갖고 있으며 여러가지 상황에 대응 하거나 퍼블리셔와 소통하기 위해 html css를 잘 알고 있어야 한다. 사실 퍼블리셔가 없는 회사가 더 많기도 하다

최근에는 flex와 grid 레이아아웃에 대해 공부중이다. 굉장히 좋은 레이아웃 시스템이다. 모든 브라우저에서 지원 되지 않은 점은 너무 아쉽다 (iE)

html / css ... 어떻게 보면 프론트엔드 개발자를 상징하는 기술명이지 않을까 싶다

  <img src="https://mspoweruser.com/wp-content/uploads/2018/12/internet-explorer.jpg" />

<i>IE 파르르르르르....</i>

> via https://mspoweruser.com/

## 브라우저의 동작원리

안드로이드앱을 개발할때는 안드로이드 OS에 대해서 잘 알고 있어야 하고, iOS앱을 개발할때는 iOS OS에 대해서 잘 알고 있어야 한다. 우리가 보는 웹페이지는 데스크탑 브라우저 또는 모바일 웹 브라우저에도 구동되며 다음과 같이 많은 종류의 브라우저가 있다 (흔히 사용되는 브라우저)

- 크롬
- 사파리
- 파이어폭스
- 네이버 웨일
- 오페라
- 인터넷 익스플로어
- 마이크로소프트 엣지
- 삼성인터넷 (모바일 브러우저)

위처럼 많은 브라우저가 기기종류에 따라 또는 유저의 성향에 따라 사용되고 있으며, 우리가 작성한 css가 특정 브라우저에서는 지원이 되고 어떤 브라우저에선 지원이 안될 수 있다 [css호환성을 확인 할 수 있는 사이트](https://caniuse.com/)

css의 호환성뿐만 아니라 개발을 하다보면 특정기능이 이 브라우저에서는 되고, 저 브라우저에서는 안되는 경우가 왕왕 발생하는데 이런문제를 조기에 찾아내고 대응하기 위해서는 브라우저가 기본적으로 어떤방법으로 구동되고, 또 IE나 삼성인터넷 브라우저같은 약간 특이한(?) 브라우저에서는 이런 기능들이 어떻게 다르게 적용되는지 알아아 할 필요가 있다

결정적으로 브라우저가 어떻게 구동되고 화면을 어떻게 렌더링 하는지 알고 있으면 `react`나 `vue`같은 js 라이브러리(또는 프레임워크)에서 벗어나 렌더링 최적화 또는 라이브러리에서 잡아 낼 수 없는 버그등을 수정 할 수 있을 것이다

## 네트워크

가끔씩 로컬호스트 환경에서 개발을 하다보면 속을 썩히는 놈이 하나 있었다. 바로 `CORS(Cross-Origin Resource Sharing)` 였는데 이 문제를 만나면 어떻게 해결방법을 찾으려고 노력하기 보다는 마음 편하게 "서버에서 풀어줘야 하는 문제" 로만 생각하고 이런 현상이 왜 생기는지 알려고 노력하지 않았었다. 하지만 굳이 서버에서 처리 해주지 않아도 브러우저에서 처리 할 수 있는 크롬 익스텐션([링크](https://chrome.google.com/webstore/detail/allow-cors-access-control/lhobafahddgcelffkeicbaginigeejlf))을 발견하게 되었고 어떤식으로 cors 이슈가 왜 생기는 것이고 어떻게 하면 좀 더 웹개발을 능숙하게 할 수 있을까에 대해서 고민하게 되었다

일단 `CORS(Cross-Origin Resource Sharing)` 에 대해서 공부를 하기 위해 [evan-moon님의 블로그](https://evan-moon.github.io/2020/05/21/about-cors/)에서 천천히 공부를 시작했다. 생각보다 알아야 할게 많았고 CORS문제 이전에 네트워크를 더욱 정확하게 공부해야 할 필요성을 느꼈다. 이미 네트워크 공부를 많이 했더라면 CORS에러를 만났을때 좀 더 여유롭게 대처 할 수 있지 않았을까??

http 통신, 소켓통신, 또 최근에는 `WEB RTC 기술`까지 수박 겉핥기식 공부로 넘어갈 수 없는 너무나도 중요한 기술이다

## 자바스크립트

이제 더 이상 자바스크립트는 프론트엔드 개발자를 상징하는 언어가 아니다. 노드의 등장으로 자바스크립트 런타임 환경이 구성되고 현재는 많은 회사에서 `Node`와 자바스크립트를 서버 개발 도구로 사용중이다.

<img src="https://velopert.com/wp-content/uploads/2016/02/nodejs-2560x1440-950x534.png" width="100%" height="auto"/>

> via https://velopert.com

하지만 내가 여기서 말하는 순수하게 브라우저에서 돌아가는 흔히 말하는 `바닐라 자바스크립트`이다. `React vs Vue vs Angular` 한 번쯤은 봤을만한 문구이고 js라이브러리(프레임워크)의 장단점을 비교하는 글은 조금만 검색해 보면 쉽게 볼 수 있다.

<img src="https://themexpert.b-cdn.net/images/easyblog_articles/688/Blog-banner.png" width="100%" height="auto"/>

> via https://www.themexpert.com/blog/angular-vs-react-vs-vue

하지만 위에서 말하는 유명한 라이브러리(또는 프레임워크)도 사실상 모두 html / css / js를 그리는 고도화된 도구일 뿐이다. 결국 브라우저가 이해하는 내용은 모두 우리가 기본적으로 알아야할 js와 html css로 구성 되어있을 것이다.

앞으로도 프론트엔드를 표현할 js 라이브러리는 계속 나올 것이고 결국 브라우저상에서 js가 어떻게 동작하는지 알아야 새로운 라이브러리에 쉽게 적응하고 사용할 수 있고, 닥치는 여러가지 문제도 쉽게 해결 할 수 있을 것이다.

물론 `React` 또는 `Vue`를 잘 아는것도 어려운 일이고 열심히 연구하고 공부해야할 분야이다 하지만 js를 더 잘 이해하고 사용한다면 `React`나 `Vue`같은 라이브러리를 더욱 다양한 생각과 시각으로 사용할 수 있을 것이라고 생각한다

## 웹팩 / gulp 등의 프론트엔드 개발 도구들

`create-react-app` 으로 만든 SPA 기반의 웹에서 IE를 지원해아 하는 일이 생겼었다. 결국 폴리필(polyfill)을 설치해야헸고 눈물을 머금고 그토록 하기 싫었던 `yarn eject`를 실행했다

<img src="https://media.giphy.com/media/AP8IYd4EAqY8M/giphy.gif" width="100%" height="auto"/>

> via GIPHY

숨겨놨던 복잡한 설정들이 쏟아져 나왔고, 구글링을 해가며 찾은 여러가지 폴리필(polyfill)을 웹팩안에 꾸겨 넣었다. "내가 뭘 하고 있는걸까" 라는 생각뿐이었다. 결국 외국의 착하고 능력있으신 개발자 형님들의 말대로 코드를 복사해서 웹팩 안 설정들을 바꿔 나갔고 내가 원하는 100%의 모습은 아니었지만 어떻게 구형 브라우저까지 지원하는 프로덕트를 서비스 할 수 있었다

구인 사이트의 프론트엔드 채용공고를 보면 `우대사항`에 거의 10분의 9확률로 있는 `webpack`에 관한 지식. 간단한 사이드 프로젝트가 아니라면, 회사가 성장하기 위해서는 반드시 프론트엔드 개발자는 `webpack`을 능숙하게 다룰 줄 알아야 할 것이다

`gulp`는 또한 비슷한 개념이다. 흔히 task runner라고 부르는 이 도구를 사실 나는 잘 모르고 있었다. 단순하게 scss를 css로 컴파일하고, `watch`를 걸어 실시간으로 코드가 반영되고 빌드 되도록 설정했던 경험만 있을 뿐이다. 그래서 두 개의 차이를 찾아봤다

> gulp vs Webpack: What are the differences?

> What is gulp? The streaming build system. Build system automating tasks: minification and copying of all JavaScript files, static images. More capable of watching files to automatically rerun the task when a file changes.

> What is Webpack? A bundler for javascript and friends. A bundler for javascript and friends. Packs many modules into a few bundled assets. Code Splitting allows to load parts for the application on demand. Through "loaders" modules can be CommonJs, AMD, ES6 modules, CSS, Images, JSON, Coffeescript, LESS, ... and your custom stuff.

> gulp and Webpack can be primarily classified as "JS Build Tools / JS Task Runners" tools.

> "Build speed", "Readable" and "Code-over-configuration" are the key factors why developers consider gulp; whereas "Most powerful bundler", "Built-in dev server with livereload" and "Can handle all types of assets" are the primary reasons why Webpack is favored.

> gulp and Webpack are both open source tools. Webpack with 49.8K GitHub stars and 6.27K forks on GitHub appears to be more popular than gulp with 31.3K GitHub stars and 4.41K GitHub forks.

> Airbnb, Instagram, and Pinterest are some of the popular companies that use Webpack, whereas gulp is used by Mailgun, Sellsuki, and Movielala. Webpack has a broader approval, being mentioned in 2206 company stacks & 1338 developers stacks; compared to gulp, which is listed in 1163 company stacks and 705 developer stacks.

> via https://stackshare.io/stackups/gulp-vs-webpack

짧은시간안에 둘의 차이점을 볼 수 있는 글을 찾아봤다(더 좋은 블로그도 있을것이다) 위의 내용만 봐서는 `gulp` 또한 충분히 프로덕트를 낼 수 있는 좋은 개발도구로 보인다. 하지만 위에서 볼 수 있듯이 `webpack`을 사용한다면 `gulp`에서 할 수 있는 일 뿐만 아니라 더 다양하고 효율적인 일들이 가능해 보인다. 실제로 많이 사용되는 `next.js` 프레임워크나 `create-react-app`안에서도 `webpack`을 사용하고 있는 것으로 보아 확실히 트렌드의 방향은 정해진 것 처럼 보인다. 물론 무조건 유행을 따를 필요는 없다. 작업하고 있는 프로젝트에 맞는 빌드도구 또는 번들러를 찾는것도 중요한 능력이다

## AWS

쌩뚱맞게 왜 `AWS`를 썼을까? 마지막 타이틀로 써야할지 말아야할지 고민이 많았다. 그리고 아직도 잘 모른다. 프론트엔드 개발자들에게는 반감이 드는 타이틀일 수도 있다. 회사의 프론트엔드팀에서 회의를 할때 항상 듣는 용어가 있다.

`cloudfront`, `route53`, `ec2`, `ecs` 등등 (사실 더 많은 aws 서비스 이름이 나오는데 내가 몰라서 기억을 못한다...) CDN서비스를 위해서는 `cloudfront`를 알아야 하고 적절한 도메인을 설정하기 위해서는 aws에서 제공하는 DNS 서비스 `route53`을 알아야 한다. 그리고 next.js로 만든 프로젝트를 배포하기 위해서는 서버사이드를 컨트롤 할 수 있는 `ec2`를 띄워야 하고 이 친구가 뭘하는 친구인지 알아야 한다. 물론 회사에서 AWS를 사용하지 않는다면 앞에서 말한 이름은 몰라도 된다. 하지만 같은 개념의 다른 이름의 서비스는 언젠간 분명히 사용 될 것이므로 이해하고 있어야 한다

## 결론

또 필요한 공부가 없을까 생각하다가 이제 겁이나서 못쓰겠다...(ui / ux에 대한 이해, CS, 자료구조 같은 것도 있겠지?) 대책없이 프론트엔드 개발이 쉬워 보이던 내가 참 한심스럽다. 앞으로도 새로운 기술들은 계속해서 생겨날 것이고 우리의 머리를 아프게 할 것이다. "이게 왜 그럴까" 하는 공학자의 마음과 결국 기본기가 가장 중요하다는 내 나름의 결론을 지어본다.

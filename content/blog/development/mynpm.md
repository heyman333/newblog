---
title: 'npm 패키지 만들어 배포해보기'
date: 2020-07-25 04:21:00
category: etc'
draft: true
---

최근에 회사에서 사용하는 소스를 모듈화 해서 npm에 배포했습니다
[깃헙링크](https://github.com/heyman333/react-native-animated-numbers)

대단한 일은 아니지만 혹시 이런 일을이 어렵게만 느껴지시는 분들을 위해 포스트를 작성합니다. 이게 별로 어려운일이 아니라는 것을 많은 자바스크립트 개발자 분들이 아셨으면 좋겠습니다 🙃

## 1. pacakage.json 생성하기

먼저 프로젝트를 생성하기 위해 `yarn init` 또는 `npm init`을 실행해줍니다. 그러면 다음처럼 간단하게 어떤 내용으로 프로젝트 또는 패키지를 만들지 설정할 수 있습니다

![yarn-init](images/yarn_init.png)

그렇다면 위에 내용으로 `package.json`파일이 생성이 되고, 우리가 제작한 모듈을 npm에 배포하기 위해서는 다음과 같은 추가 정보가 더 필요합니다.

```json
{
  "name": "react-native-animated-numbers",
  "version": "0.0.3",
  "main": "index.js",
  "typings": "index.d.ts",
  "description": "Module that shows number changes through simple animation",
  "scripts": {
    "lint": "yarn eslint .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/heyman333/react-native-animated-numbers"
  },
  "keywords": [
    "react-native",
    "react-native-component",
    "react-component",
    "ios",
    "android",
    "react",
    "animation",
    "text",
    "number"
  ],
  "author": "youngsoohan",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/heyman333/react-native-animated-numbers/issues"
  },
  "homepage": "https://github.com/heyman333/react-native-animated-numbers/blob/master/README.md",
  "peerDependencies": {
    "react": ">=16.8.0",
    "react-native": ">=0.59"
  },
  "devDependencies": {
    "eslint": "^7.4.0",
    "eslint-plugin-react": "^7.20.3"
  }
}
```

- typings: typescript 사용을 위한 타입 정의 파일 지정
- scripts: 자주 사용하는 cli명령을 지정
- keywords: https://www.npmjs.com/ 내에 키워드를 명시하기 이해 사용
- peerDependencies: peerDependencies를 명시 (명시된 모듈 위에서 동작함을 명시)
- devDependencies: devDependencies를 명시 (해당 모듈을 개발하기 위해서 다음 모듈이 필요함을 명시)

여기까지 완성이 되면 npm에 배포할 기본 자격은 맞춰졌습니다

## 2. index.js 파일에 내가 만든 소스 작성하기

이제 실질적으로 사용되는 소스를 여기에 작성하시면 됩니다. 제가 오래전에 만든 간단한 라이브러리의 소스를 옮겨보곘습니다.

### index.js

```js
import { isIphoneX } from 'react-native-iphone-x-helper'
import { Platform, StatusBar, Dimensions } from 'react-native'

const { height, width } = Dimensions.get('window')
const standardLength = width > height ? width : height
const offset =
  width > height ? 0 : Platform.OS === 'ios' ? 78 : StatusBar.currentHeight // iPhone X style SafeAreaView size in portrait

const deviceHeight =
  isIphoneX() || Platform.OS === 'android'
    ? standardLength - offset
    : standardLength

export function RFPercentage(percent) {
  const heightPercent = (percent * deviceHeight) / 100
  return Math.round(heightPercent)
}

// guideline height for standard 5" device screen is 680
export function RFValue(fontSize, standardScreenHeight = 680) {
  const heightPercent = (fontSize * deviceHeight) / standardScreenHeight
  return Math.round(heightPercent)
}
```

타입스크립트를 지원한다면 아래와 비슷한 식으로 package.json에 정의된 대로 type을 정의해주시면 됩니다

### index.d.ts

```ts
declare function RFPercentage(percent: number): number
declare function RFValue(value: number, standardScreenHeight?: number): number

export { RFPercentage, RFValue }
```

## 3. npmignore 작성하기

npmignore은 깃헙등 레포지토리에는 표시가 되지만 실제 다운로드 되는 `node_modules` 에는 다운로드가(배포가) 되지 않도록 제외시킬 목록을 정의하는 파일입니다.
보통은 `.git`이 포함되며 `images` 또는 예제 프로젝트 `examples` 등이 포함됩니다 파일명은 `.npmignore`로 숨김처리 하여 만듭니다

```
ExampleProject
images
.circleci
.git
```

## 4. npm 계정 만들고 배포하기

npm 이나 yarn 명령어로 다른 개발자가 내 모듈을 설치 하기 위해서는 npm 레지스트리에 내가 만든 소스가 올라가야 되고 npm 계정이 있어야 합니다.
먼저 https://www.npmjs.com/signup 에서 npm 계정을 만듭니다. 그리고 다음처럼 `npm login`으로 터미널에서 로그인을 진행합니다

```shell
> npm login

Username: youngsoohan
Password:
Email: (this IS public) amazingmobdev@gmail.com
Logged in as youngsoohan on https://registry.npmjs.org/.
```

배포는 `npm publush` 명령어로 진행이 되는데 모듈의 버전은 다음처럼 `package.json`에 명시된 버전이 됩니다

```json
// example
"version": "0.4.3",
```

- 명령어 실행

```shell
> npm publish

npm notice
npm notice 📦  your-package-name@0.0.4
npm notice === Tarball Contents ===
npm notice 1.1kB LICENSE
npm notice 532B  .eslintrc.js
npm notice 2.9kB index.js
npm notice 1.0kB package.json
npm notice 2.7kB README.md
npm notice 363B  index.d.ts
npm notice === Tarball Details ===
npm notice name:          react-native-animated-numbers
npm notice version:       0.0.4
npm notice package size:  3.5 kB
npm notice unpacked size: 8.6 kB
npm notice shasum:        25c7270b68521655ccbcbf60a936244ac35d0f7c
npm notice integrity:     sha512-MkEXb3f5SXxKa[...]/rp593FjZ94fA==
npm notice total files:   6
npm notice
+ your-pacakge-name@0.0.4
```

## 5. 그 외 있으면 좋은 것들

1.  `circleci` 등 ci 설정하기

    소스를 레포에 올린 뒤에 누군가가 pull reqeust를 보낼 수도 있고, 내가 feature를 따서 작성한 뒤에 마스터에 머지하는 작업등을 할 수 있습니다.
    이 때 자동으로 내 코드의 `lint`를 잡아주거나 `test 코드 실행`을 자동화 도구를 사용할 수 있는데 대표적인 무료툴은 [circleci](https://circleci.com/)가 있습니다(오픈소스 한정)

![example](images/circleci.png)

> 깃헙에서 circleci로 pull request를 검증하는 예시(React-Native repo)

circleci 홈페이지에서 가입을 하고 다음처럼 내가 설정하려는 레포가 포함된 organization을 설정합니다(보통은 자기 계정이 되겠죠?)

![org](images/org.png)

그리고 다음처럼 내가 circleci config를 넣고 싶은 레포에서 다음처럼 `ylm`파일을 설정하면

![ylm](images/ylm.png)

아래 https://github.com/heyman333/TIL 레포에서 확인할 수 있는 것 처럼 `yml`파일이 설정된 것을 보실 수 있습니다.

![github_config](images/github_config.png)

`yml`파일을 정의하기 위해선 별도의 문법을 따로 공부해야 합니다. 비교적 복잡하고 어려운 workflow가 동작하길 원한다면 시간을 내서 문서등을 찾아보고 공부하실 것을 추천 드립니다 🙃

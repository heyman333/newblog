---
title: '러나 (Lerna) 훑어보기'
date: 2020-02-02 21:56:00
category: 'etc'
draft: false
---

러나는 하나의 저장소에서 여러자의 패키지를 구성하는 것을 도와주는 라이브러리입니다 `eslint`, `jest`, `typescript` 등의 설정파일은 보통 각각의 별도의 패키지에서 공유하는 내용이므로 루트디렉토리에 존재하게 됩니다. 대표적으로 러나를 사용하고 있는 프로젝트틑 `babel`이 있습니다(https://github.com/babel/babel)

`babel`프로젝트를 보시면 아시겠지만 이 역시 공통적으로 사용되는 설정파일은 모두 루트디렉토리에 포함해서 사용하고 있는것을 볼 수 있습니다.
러나를 사용하고 있는 프로젝트에는 기본적으로 루트에 `lerna.json`파일이 존재합니다

다음 코드는 `babel`프로젝트에 존재하는 `lerna.json`의 일부인데요

```json
  "packages": [
    "codemods/*",
    "eslint/*",
    "packages/*"
  ],
  "npmClient": "yarn"
```

`package` 키는 각각으로 독립적으로 존재하는 패키지를 정의할 때 사용되고, `npmClient`는 `node_modules` 매니저 관리도구를 명시 할때 사용합니다. 아무것도 설정하지 않으면 기본적으로 `npm`을 사용합니다

react-native-seoul 밋업에서 진행중인 dooboo-ui-native (https://github.com/dooboolab/dooboo-ui-native) 프로젝트를 보면

```json
  "packages": [
    "src/components/shared/*"
  ],
```

위처럼 각각의 독립적인 패키지가 `"src/components/shared/*"`에서 존재하는 것을 볼 수 있고, 폴더 아래에는 각각 `package.json`파일을 가진 별도의 프로젝트가 독립적으로 운영되고 있는 것을 확인 할 수 있습니다. 이렇게 각각의 패키지는 `yarn add @dooboo-ui/native-snackbar`처럼 별도로 모듈설치 또는 배포가 가능합니다

# 모노레포와 러나를 사용하는 이유

그렇다면 이렇게 모노레포와 러나 라이브러리를 이용해서 얻을 수 있는 이점은 무엇이 있을까요?

- 공통된 설정파일 작성

  - 각각의 패키지에는 모든 설정파일을 공유 할 수 있습니다. 물론 패키지 따로따로 설정파일을 따로 지정 할 수 있지만 보통의 경우에는 모든 설정파일을 루트 디렉토리에 저장하고 있고 각각의 패키지가 그 설정파일을 공유하는 구조로 이루어져 있습니다

- 패키지 공유

  - 위에서 말한 것 처럼 각강의 패키지는 각자의 디렉토리에서 따로 배포 될 수 있습니다. 하지만 러나에서 제공하는 `bootstrap`(https://github.com/lerna/lerna/tree/master/commands/bootstrap) 명령어를 실행하면 각각의 다른 패키지에서 마치 그 패키지가 `node_modules`에 포함되어있는 것처럼 사용 할 수 있습니다. babel 프로젝트를 예시로 보겠습니다 `babel/packages/babel-cli/src/babel/util.js` 파일을 보면
    ```ts
    import readdirRecursive from 'fs-readdir-recursive'
    import * as babel from '@babel/core'
    import includes from 'lodash/includes'
    import path from 'path'
    import fs from 'fs'
    ```
    위처럼 `@babel/core` 패키지를 가져와서 사용하는 것을 볼 수 있습니다. 만약에 `@babel/core` 가 실제로 배포 되어 있지 않더라도 다른패키지에서 편하게 `import`해서 사용할 수 있습니다👍

- 편한 패키지 관리와 소스참조

  - 언제부터인가 `react-navigation` 의 코어 프로젝트가 모두 분리 되면서 https://github.com/react-navigation/stack 등과 같이 레포를 다른 주소에서 따로 참조해야 하는 불폄함이 생겼습니다. 하지만 모노레포를 사용하면 패키지를 따로 관리 할 수도 있고, 하나의 레포에서 소스 모두를 볼 수 있으므로 편리함을 느낄 수 있습니다

# 자주 사용되는 러나 명령어

- lerna clean
  - 모든 패키지의 노드모듈을 삭제합니다. 루트 디렉토리의 노드모듈은 삭제하지 않습니다
  - 클린빌드의 목적으로 사용할 수 있습니다
  - https://github.com/lerna/lerna/tree/master/commands/clean

* lerna bootstrap

  - 모든 패키지에 명시된 모듈을 설치합니다
  - 위에서 설명한 것처럼 다른 패키지에서 다른 패키지를 참조 할 수 있도록 링킹하는 작업을 진행합니다
  - https://github.com/lerna/lerna/tree/master/commands/bootstrap

* lerna run `script`

  - 각각에 페키지에 명시된 `package.json` 의 `script` 를 실행합니다
  - https://github.com/lerna/lerna/tree/master/commands/run

* lerna publish

  - 패키지의 배포를 관리 하는데 사용됩니다
  - 아무 `agrument`를 사용하지 않을경우 모든 패키지의 버전을 `lerna.json`에 명시된 버전으로 업데이트 후 배포를 진행합니다(v 2.0.0)

    ```script
    - @dooboo-ui/native-accordion: 1.5.5 => 2.0.0
    - @dooboo-ui/native-button-group: 0.4.0 => 2.0.0
    ```

  - `lerna publish from-package`을 사용할 경우 각각의 패키지의 버전과 `npm 레지스트리`에 실제로 배포되어있는 버전을 비교하고 차이점이 있는 패키지만 업데이트 후 배포를 진행합니다

  - `lerna publish from-git`을 사용할 경우 태그를 달은 커밋에 해당되는 소스가 포함되어 있는 패키지를 찾아서 업데이트 후 배포를 진행합니다

# 참고

- https://medium.com/jung-han/lerna-로-모노레포-해보러나-34c8e008106a
- https://github.com/lerna/lerna

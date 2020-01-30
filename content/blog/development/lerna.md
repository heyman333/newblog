---
title: '러나 (Lerna) 훑어보기'
date: 2020-01-31 00:56:00
category: 'etc'
draft: true
---

러나는 하나의 저장소에서 여러개의 패키지를 구성하는 것을 도와주는 라이브러리입니다 `eslint`, `jest`, `typescript` 등의 설정파일은 보통 각각의 별도의 패키지에서 공유하는 내용이므로 루트디렉토리에 존재하게 됩니다. 대표적으로 러나를 사용하고 있는 프로젝트틑 `babel`이 있습니다(https://github.com/babel/babel)

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

`package` 키는 각각으로 독립적으로 존재하는 패키지를 정의할 때 사용되고, `npmClient`는 `node_modules` 매니저 관리도구를ㄴ 명시 할떄 사용합니다. 아무것도 설정하지 않으면 기본적으로 `npm`을 사용합니다

---
title: '프론트개발자가 만드는 노드서버'
date: 2020-05-03 14:30:00
category: 'node'
draft: true
---

회사에서 평생 프론트 개발만 하다가 "나도 혼자 서비스를 만들어 보고 싶다" 라는 마음에 슬슬 노드 공부를 하고 있습니다 이 포스트에서는 어떤식으로 초보 서버 개밸자가 프로젝트를 구성하고 진행하고 있는지 공유 하려고 합니다. 이제 막 서버 개발을 시작하시려는 분들에게 조금이라도 도움이 되었으면 좋겠습니다👍

프로젝트에는 다음 기술을 사용했습니다

- express (노드 서버 프레임워크)
- sequelize (db orm)
- mysql (db)

## 디렉토리 구조

프로젝트를 시작할떄 가장 먼저 드는 생각은 어떤식으로 파일과 폴더를 분리 하는 것일까 하는 고민일 것입니다 저는 다음 처럼 폴더와 파일을 분리 했습니다

```terminal
├── .babelrc
├── .env
├── .eslintrc.json
├── .prettierrc
├── README.md
├── package.json
├── src
│   ├── api // 엔드포인트
│   │   ├── index.js
│   │   ├── middlewares
│   │   │   ├── index.js
│   │   │   └── isAuth.js
│   │   └── routes
│   │       └── auth.js
│   ├── app.js // 엔트리
│   ├── config
│   │   ├── config.sequelize.js
│   │   └── index.js
│   ├── loaders
│   │   ├── container.js
│   │   ├── express.js
│   │   ├── index.js
│   │   └── logger.js
│   ├── migrations // sequelize cli에서 생성해준 migrations 폴더
│   ├── models // sequelize cli에서 생성해준 models 폴더
│   │   ├── article.js
│   │   ├── index.js
│   │   └── user.js
│   ├── seeders // sequelize cli에서 생성해준 seeders 폴더
│   └── services // 비지니스 로직
│       └── auth.js
└── yarn.lock
```

[sequelize cli](https://github.com/sequelize/cli)를 이용하면 [sequelize orm](https://github.com/sequelize/sequelize)을 사용하기 위한 기본적인 폴더와 파일을 다음처럼 생성해 줍니다

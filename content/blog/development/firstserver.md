---
title: '프론트개발자가 만드는 노드서버'
date: 2020-05-04 04:00:00
category: 'node'
draft: false
---

회사에서 프론트 개발만 하다가 "나도 혼자 서비스를 만들어 보고 싶다" 라는 마음에 슬슬 노드 공부를 하고 있습니다. 이 포스트에서는 서버개발을 하면서 정리하고 싶은 내용과 앞으로 개발을 진행하면서 생기는 시행착오를 정리합니다.

기술스택은 다음과 같습니다

- express (노드 서버 프레임워크)
- sequelize (orm)
- mysql (db)

## 프로젝트 구조

프로젝트를 시작할 때 가장 먼저 드는 생각은 어떤식으로 파일과 폴더를 분리 하는 것일까 하는 고민일 것입니다 저는 다음 처럼 폴더와 파일을 분리 했습니다

```terminal
├── .babelrc
├── .env
├── .eslintrc.json
├── .prettierrc
├── README.md
├── package.json
├── src
│   ├── api // routes
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

```terminal
Created "config/config.json"
Successfully created models folder at "project root/models".
Successfully created migrations folder at "project root/migrations".
Successfully created seeders folder at "project root/seeders".
```

```terminal
├── config
│   └── config.json
├── migrations
├── models
│   └── index.js
├── seeders
```

위처럼 폴더와 파일을 자동으로 만들어주면 상당히 편하지만 보통 src폴더를 만들고 그 밑에 개발과 관련된 소스들을 모아두기 때문에 저는 별도로 src 폴더에 위에서 생성된 파일을 모두 옮겨 넣었습니다. 제 기준으로 디렉토리 구조를 구성할때 신경써야 할 점은 다음이었습니다

1. 비지니스 로직과 routes의 분리
2. 데이터베이스 모델의 분리
3. express 미들웨어와 error handler의 분리

데이터베이스 모델의 분리는 `sequelize`의 도움을 받아 비교적 손쉽게 진행 할 수 있었고, 전체적인
프로젝트 구조를 구성하는데에는 [이 기술 블로그](https://softwareontheroad.com/ideal-nodejs-project-structure/)가 상당히 많은 도움이 되었습니다

실제 프로젝트 구성: https://github.com/heyman333/youngstargram-node-server

## 필수 및 유용한 라이브러리 (진행중 업데이트 예정)

### 인증 및 암호화

1. crypto (node 내장 모듈)

   - 암호화를 위해 사용하는 노드 내장 모듈, 보통 유저의 비밀번호를 암호화 해서 DB에 저장하는데 사용. 그 외 암호화가 필요한 모든 데이터에 사용 될 수 있다

2. jsonwebtoken (https://github.com/auth0/node-jsonwebtoken)

   - 흔히 말하는 `jwt` 토큰 발급 및 검증, 그리고 decode 등을 위해서 사용하는 라이브러리

### 유효성 검토

1. celebrate (https://github.com/arb/celebrate)

   - req.params, req.headers, req.query 등 클라이언트의 요청이 제대로 된 요청인지 DB처리 과정 또는 서비스 로직을 처리하기 전에 확인 하기 위해 사용하는 라이브러리

   example:

   ```js
   app.use(
     celebrate({
       headers: Joi.object({
         token: Joi.string()
           .required()
           .regex(/abc\d{3}/),
       }).unknown(),
     })
   )
   ```

### 이미지 업로드

1. multer (https://github.com/expressjs/multer)

   - 파일 업로드를 위해 사용되는 multipart/form-data 를 다루기 위한 미들웨어
   - 파일 한개 및 여러개 업로드를 위한 편리한 메소드를 제공

2. multer-s3 (https://github.com/expressjs/multer)

   - multer를 이용해서 aws s3에 이미지를 올리기 용이하게 지원하는 라이브러리
   - s3를 config할 수 있는 여러가지 옵션을 제공

## TODO

1. AWS S3와 CloudFront 연동 및 이미지 저장
2. 태그 기능 구현
3. 프론트엔드 (React Native or React web)
4. ~~브레이크포인트로 디버그 할 수 있도록 프로젝트 설정~~

## 참고

1. https://expressjs.com/ko/
2. https://sequelize.org/

---
title: 'let, const는 호이스팅 되지 않을까?'
date: 2020-12-14 12:00:00
category: 'javascript'
draft: true
---

let, const 라는 키워드가 es6에 새로 생겨나고 변수를 라인 밑에서 선언하고 그 위에서 참조 할 수 없다는 이유만으로 `호이스팅`이 되지 않는다고 오해하고 말하고 다녔습니다. 하지만 결론적으로 자바스크립트는 ES6에서 도입된 let, const를 포함하여 모든 선언(var, let, const, function, function\*, class)을 호이스팅 합니다.

잘못 알고 있었던 내용과 함께 관련 내용을 정리하려고 합니다.

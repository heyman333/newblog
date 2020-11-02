---
title: '자바스크립트 클로저(Closure) 이해하기'
date: 2020-11-02 04:00:00
category: 'javascript'
draft: true
---

프론트엔드 직군 면접 필수 질문 자바스크립트 `클로저(Closure)` 를 이해하기 위해 정리합니다.

클로저를 가장 쉽게 이해할 수 있는 말은 이게 아닐까 쉽습니다

```
클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경을 ‘기억한다’.
```

MDN에서 클로져를 소개하고 있는 글입니다.

클로저를 사용하면서도 이게 클로저를 사용하는 것인지 생각지도 못하고 사용하게 되는 경우가 있었는데 바로 다음과 같은 예입니다

```js
function showName() {
  const name = 'hys'
}
```
---
title: '자바스크립트는 브라우저에서 어떻게 동작하는가'
date: 2020-11-05 04:00:00
category: 'javascript'
draft: true
---

많은 분들이 알다시피 javacrtipt는 싱글스레드 환경에서 구동되는 언어입니다. 하지만 자바스크립트에서는 이벤트루프라는 매커니즘을 동작시켜 여러개의 비동기 함수와 즉시실행되는 함수를 일정한 조건과 순서에 맞게 실행해 마치 여러개의 스레드가 동작하는 것처럼 보입니다.

그러면 다음 그림을 보고 전체적인 구조를 봅시다

<img src="images/js_engine.png" width="100%" height="auto" />

> 이미지 출처: https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf

왼쪽 박스 안은 `자바스크립트엔진` 이며 다음으로 구성 되어 있습니다

### 자바스크립트엔진

1. memory heap
   - 변수와 함수사용을 위한 메모리가 할당되는 영역
2. call stack
   - 코드실행이구문이 말그대로 stack으로 쌓이는 곳
   - LAST IN FIRST OUT (FIFO) 방식으로 실행
   - 자바스크립트가 싱글스레드라고 불리는 이유는 콜스택이 하나이기 때문

`자바스크립트엔진` 외부에는 `Web APIs` & `Callback Queue` & `Event loop`가 있습니다.

### Web APIs

자바스크립트 엔진에서 제공하는 기능이 아닙니다. `DOM`, `AJAX`, `Timeout` 등 우리가 자주 사용하는 이 기능은 모두 브라우저에서 제공해주는 브라우저만이 가지는 기능입니다. 하지만 위 기능들도 결국 `call stack`안에서 실행 될 것이며, 그 안에서 실행되는 콜백함수는 모두 위의 그림에 나와있는 콜백큐(Queue)안에 들어갑니다.

### Callback Queue

위에서 말한것처럼 비동기 함수들이 보관되는 영역입니다. `setTimeout(callback, time)`, `addEventListener` 안에서 사용되는 콜백을 생각 할 수 있습니다.

ex:

```js
someElement.addEventListener(
  'mouseup',
  handleMouseUp, // callback
  passiveSupported ? { passive: true } : false
)
```

### Event loop

그림에서 보이듯이 자바스크립트 엔진안의 CallStack이 `모두 비어있는지` 확인하고 Callback Queue안의 콜백을 CallStack안에 넣고 실행합니다. 여기서 콜스택이 `모두 비어있는지`를 확인하고 콜백을 스택안에 넣는다는 것이 중요합니다. 이 과정을 틱(Tick)이라고 부릅니다.

### Microtask Queue

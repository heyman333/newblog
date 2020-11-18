---
title: '자바스크립트는 브라우저에서 어떻게 동작하는가'
date: 2020-11-05 04:00:00
category: 'javascript'
draft: false
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

모든 콜백함수가 코드구문의 순서대로 실행되는 것이 아닙니다. 다음 그림을 보겠습니다.

<img src="images/browser-structure.png" width="100%" height="auto" />

> 이미지 출처: http://sculove.github.io/blog/2018/01/18/javascriptflow/

그림을 보면 Callback queue가 세개로 나뉘어 있다는것을 알 수 있습니다. 이벤트루프는 이 세개의 Queue를 분리해서 어떤 콜백함수를 먼저 실행할지 우선순위에 따라 콜스택으로 함수를 보냅니다.

그중 가장 우선 순위가 높은 Queue는 바로 `Microtask Queue`입니다. 다음 코드를 https://playcode.io/ 에서 실행해보겠습니다.

```js
console.log('script start')

setTimeout(function() {
  console.log('setTimeout')
}, 0)

Promise.resolve()
  .then(function() {
    console.log('promise1')
  })
  .then(function() {
    console.log('promise2')
  })

console.log('script end')
```

result:

```js
// console

script start
script end
promise1
promise2
setTimeout
```

`setTimeout`의 두 번째 인자가 0으로 넘어갔으니 바로 실행이 되지 않을까?라고 생각하기 쉬운 부분입니다. 심지어 `setTimeout`은 `Promise`가 끝나고 나서 불리게 됩니다. 왜 이런 결과가 나오게 되었을까요? 단순이 console을 출력하는 `console.log('script start')` 와 `console.log('script end')`는 비동기 (콜백함수)가 아니기 때문에 바로 콜스택안에 들어가 실행됩니다. 이후 바로 `Promise`가 콜스택으로 들어갑니다. 이유는 `Promise`가 콜백에서 가장 높은 우선순위를 가지는 `Microtask Queue`이기 때문입니다.

이후에 실행되는 `setTimeout`은 콜백큐에서 가장 낮은 우선순위를 갖는 `Task Queue`입니다.

### Microtask Queue vs Task Queue vs Animation Frames

위 그림에서는 총 3개의 콜백큐가 그려져 있는데요. 다음 예제를 통해 큐의 우선순위를 알아보겠습니다.

```js
//1. script 실행 (log)
console.log('script start')

//2. script 실행 (setTimeout callback task queue에 등록)
setTimeout(function() {
  //11. Task 실행
  console.log('setTimeout')
}, 0)

//3. script 실행 (Promise then callback Microtask queue에 등록)
Promise.resolve()
  .then(function() {
    // 7. MicroTask 실행
    console.log('promise1')
  }) // 8. script 실행 (Promise then callback Microtask queue에 등록)
  .then(function() {
    // 9. MicroTask 실행
    console.log('promise2')
  })

//4. script 실행 (AnimationFrame Animation frames에 등록)
requestAnimationFrame(function() {
  //10. Animation Frame 실행
  console.log('animation')
})

//5. script 실행
console.log('script end')
//6. Stack의 모든 Task 실행완료
```

```js
//console

script start
script end
promise1
promise2
animation
setTimeout
```

결론:

<b>Microtask Queue > Animation Frames > Task Queue</b>

### 출처 및 참고

- https://iamsjy17.github.io/javascript/2019/07/20/how-to-works-js.html
- https://velog.io/@thms200/Event-Loop-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84

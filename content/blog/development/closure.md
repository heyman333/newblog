---
title: '자바스크립트 클로저(Closure) 이해하기'
date: 2020-11-02 04:00:00
category: 'javascript'
draft: false
---

클로저를 가장 쉽게 이해할 수 있는 말은 이게 아닐까 싶습니다

```
클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경을 ‘기억한다’.
```

MDN에서 클로져를 소개하고 있는 글입니다.

클로저를 사용하면서도 이게 클로저를 사용하는 것인지 모르고 단순히 경험에 의한 감으로 클로저를 사용할 경우가 있었는데 다음과 같은 경우입니다

```js
function showName() {
  const name = 'youngsoo han'

  const printFunction = () => {
    console.log('name is ' + name)
    return name
  }

  return printFunction
}

const showNameFunction = showName()
console.log('hello ' + showNameFunction())
```

결과:

```zsh
name is youngsoo han
hello youngsoo han
```

위의 코드를 보면 `showName` 함수는 이미 자기의 동작을 마치고 `printFunction` 을 리턴했습니다. 단순하게 생각한다면 `showName`안에서 생성한 변수는 변수를 할당한 주체의 생명이 다 했으니 메모리에서 사라졌다고 생각할 수 있습니다

하지만 결과값을 보면 알 수 있듯이 `showName`안에 `name`변수는 메모리에 잘 남아 있으며 이를 `printFunction`이 참조하고 있는 것을 확인 할 수 있습니다. MDN에서 소개하고 있는 것처럼 클로저 안에 정의 된 함수가 그 위의 `name`이라는 변수를 잘 기억하고 있네요

다시 한번 클로저를 이해하기 위해서 여러 블로그에서 가장 많이 보는 예제를 소개해 보겠습니다

```js
let i
for (i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i)
  }, 100)
}
```

이 역시 단순한게 생각한다면 for문안에 모든 setTimeout이 100ms뒤에 차례대로 실행되어 0 ... 9 까지 출력 될 것이라고 생각할 수 있습니다. 하지만 결과를 보면 어떨까요?

```zsh
10
10
10
10
10
10
10
10
10
10
```

이렇게 10이 10번이 찍히는 걸 알 수 있습니다. 첫번째 `setTimeout`이 불리기 전에 (100ms 전에) 이미 i는 for문을 10번을 다 돌아서 i++로 증가함에 따라 10으로 올라가 있고, 이후에 console을 통해 찍히는 i 값은 이미 모두 10입니다. 그렇다면 우리가 원하는 결과값을 얻기 위해서 위에서 학습한 클로저를 사용해 원하는 결과값을 도출해 보겠습니다.

```js
for (let i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i)
  }, 100)
}
```

또는

```js
let i
for (let i = 0; i < 10; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j)
    }, 100)
  })(i)
}
```

결과:

```zsh
0
1
2
3
4
5
6
7
8
9
```
원하는 결과값이 제대로 나오는 것을 확인 할 수 있습니다. 첫번째 경우는 변수 i값이 for문 스코프 안으로 들어왔습니다. for문 의 실행부분이 클로저로 적용 되어 `setTimeout` 안에서 실행당시 i값을 적절하게 참조합니다.

두 번째 예제에서는 클로저함수의 특징을 좀 더 명확하게 보실 수 있습니다. 즉시실행함수(IIFE)를 통해 클로저 안의 함수가 생성되는 시점에 적절하게 i값을 참조 할 수 있도록 보완 했습니다.

그렇다면 클로저 함수는 어느곳에서 유용하게 사용할 수 있을까요?


## debounce함수의 구현

```js
function debounce(func, wait, immediate) {
  let timeout;
  return function() {
  	const context = this, args = arguments;
  	clearTimeout(timeout);
  	timeout = setTimeout(function() {
  		timeout = null;
  		if (!immediate) func.apply(context, args);
  	}, wait);
  	if (immediate && !timeout) func.apply(context, args);
  };
}
```

자주 사용하는 `debounce`도 위처럼 클로저 안의 변수를 활용해서 구현할 수 있습니다 (timeout). 외부에서 사용하는 함수 는 결국 반환되는 함수이겠지만 timeout값을 계속 기억하고 있다가 clearTimeout을 통해 기존에 실행되기로한 함수를 없에는 등 의 동작을 실행합니다.

위 원리를 이용하면 비슷한 동작을 필요로 하는 여러가지 함수를 만들 수 있습니다.


## 정보의 은닉화(캡슐화)

C++ 또는 자바같은 객체지향 프로그래밍 언어를 배울 때 한번쯤은 들어본 용어입니다. 함수 또는 클래스에서 어떤 기능을 제공 할 때 외부에서 함부로 바꿀 수 없는 private한 변수값을 만들어 주고 싶다면 이때도 클로저를 활용 할 수 있습니다.

자바스크립트에서 객체지향적으로 설계를 하고 싶을 때 다음처럼 `Prototype`을 이용합니다.

```js
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function() {
  console.log('밥먹자, ' + this._name);
}

const hello1 = new Hello('강아지');
const hello2 = new Hello('고양이');
const hello3 = new Hello('망아지');

hello1.say();
hello2.say();
hello3.say();
hello1._name = '꿀꿀이';
hello1.say();
```
위에서 볼 수 있듯이 외부에 노출되지 않아도 되는 `_name` 값이 함수 밖에서 손쉽게 조작되는 것을 볼 수 있습니다. 하지만

```js
function hello(name) {
  const _name = name;
  return function() {
    console.log('밥먹자, ' + _name);
  };
}

const hello1 = hello('강아지');
const hello2 = hello('고양이');
const hello3 = hello('망아지');

hello1();
hello2();
hello3();
```

위 같이 클로저를 사용한다면 _name에 접근할 수 있는 방법을 제공하지 않고 원하는 결과 값을 얻을 수 있습니다.

# 참조
위 글은 다음 블로그의 예시를 많이 사용 하였으며 제가 클로저를 공부하는데 상당 부분 영향을 미쳤습니다.


https://hyunseob.github.io/2016/08/30/javascript-closure/
---
title: 'React key prop 이해하기'
date: 2019-11-20 20:51:00
category: 'react'
---

```
Warning: Each child in an array or iterator should have a unique "key" prop ...
```

리액트를 개발하면서 한번쯤은 보셨을 경고 메세지입니다. 이번 포스팅에서는 리액트 Element 에 왜 key prop이 필요한지 알아보고, key prop을 사용하면서 주의해야 할 점을 설명합니다

# 재조정 (Reconciliation)

`key` prop을 이해 하기에 앞서 우리는 React가 화면을 어떻게 업데이트 하는지 알 필요가 있습니다 React는 새로운 엘리먼트를 그리기 위해 [비교 알고리즘 (Diffing Algorithm)](https://ko.reactjs.org/docs/reconciliation.html#the-diffing-algorithm)을 이용해서 효율성을 최대화 합니다

위 링크에 대한 내용을 다시 한번 정리해보겠습니다

- 엘리먼트의 타입이 다른 경우

```jsx
  // before
  <div>
    <Counter />
  </div>
  // after
  <span>
    <Counter />
  </span>
```

이 경우는 아예 DOM을 새로 그리는 경우로 이전의 컴포넌트 인스턴스(Counter)는 모두 파괴되고 새로운 인스턴스가 생성됩니다(Counter) 이때 기존의 `Counter` 컴포넌트 에서는 `componentWillUnmount()`가 실행 될 것이며 새로운 인스턴스에는 `componentWillMount()` 와 `componentDidMount()`가 새롭게 실행 될 것입니다

<i>(참고: `componentWillMount`는 React 16.8 부터 deprecated 되었습니다)</i>

- 엘리먼트의 타입이 같은 경우

```jsx
  // before
  <div style={{color: 'red', fontWeight: 'bold'}} />
  // after
  <div style={{color: 'green', fontWeight: 'bold'}} />
```

이 경우는 두 엘리먼트의 타입이 동일 하기 때문에 두 엘리먼트의 속성만 비교하며 동일한 내역은 유지하고 변경된 속성값만 갱신합니다
위에서는 `fontWeight`는 수정하지 않고, `color`값만 새롭게 수정합니다 이 경우는 기존의 엘리먼트는 기존의 `state` 값을 유지 하며
새로운 속성을 반영하기 위해 컴포넌트의 props를 갱신합니다 이떄 컴포넌트에서는 이때 해당 인스턴스의 `componentWillReceiveProps()`와 `componentWillUpdate()`를 호출합니다.

<i>(참고: `componentWillReceiveProps` 와 `componentWillUpdate` 역시 react 16.8 부터 deprecated 되었습니다. 이해를 돕기 위한 설명이니 실제로는 이 라이프사이클 메소드를 사용하지 않는 것이 좋습니다)</i>

# 자식에 대한 재귀적 처리

다음 설명에서는 왜 우리가 반복되는 엘리먼트를 추가 할때 `key` prop을 넣어야 하는지 알 수 있습니다

```jsx
  // before
  <ul>
    <li>first</li>
    <li>second</li>
  </ul>
  // after
  <ul>
    <li>first</li>
    <li>second</li>
    <li>third</li>
  </ul>
```

리액트 개발을 하신분이라면 `key` prop 이 넘겨지지 않은 위같은 엘리먼트 선언이 어색하게 느껴지실겁니다. 하지만 위와 같이 마지막에 새로운 엘리먼트를 추가 하는 것은 크게 성능 문제를 유발하지 않습니다 React는 모든 자식 노드를 순회하면서 차이점이 있으면 변경을 생성하는데 위 같은 경우는 `첫 번째` `두 번째` 엘리먼트가 똑같고 마지막에 `<li>third</li>`를 추가하면 되기 때문에 모든 자식 노드를 새로 그릴 필요 없이 변경된 사항만 새롭게 그리게 됩니다

하지만 다음은 어떨까요?

```jsx
  // before
  <ul>
    <li>first</li>
    <li>second</li>
  </ul>
  // after
  <ul>
    <li>first</li>
    <li>second</li>
    <li>third</li>
  </ul>
```

새로운 엘리먼트가 마지막이 아니라 첫번째로 들어갑니다 이 경우에는 React는 모든 요소가 제자리에 위치하지 않았다고 생각하고 종속트리는 유지 하지만 모든 자식 엘리먼트를 새로 그립니다 이 경우에는 의도치 않게 성능이슈를 유발할 수 있겠지요

그렇다면 이제는 우리가 평소 하던대로 `key` prop을 넣어서 엘리먼트를 다시 생성해 봅시다

```jsx
  // before
  <ul>
    <li key="2015">Duke</li>
    <li key="2016">Villanova</li>
  </ul>

  // after
  <ul>
    <li key="2014">Connecticut</li>
    <li key="2015">Duke</li>
    <li key="2016">Villanova</li>
  </ul>
```

이렇게 `key` prop을 넘겨주면 React는 '2014' key를 가진 엘리먼트가 새로 추가되었고, '2015'와 '2016' key를 가진 엘리먼트는 그저 이동만 하면 되는 것을 알 수 있습니다.

# 실수하지 않기

대부분의 데이터 리스트 아이템은 `id` 값을 가지고 있을 것이고 어렵지 않게 `key={data.id}` 와 같이 `key` prop을 주입 할 수 있습니다 그렇지 않은 경우 대게 아이템의 `index`값을 이용해 쉽게 `key` prop을 채울 수 있다고 생각하지만 이는 잘못된 방법입니다

리액트 공식문서에 나와있는 [예제](https://codepen.io/pen?&editable=true&editors=0010)를 보겠습니다

```jsx
render() {
    return (
      <div>
        ...
        <table>
          <tr>
            <th>ID</th>
            <th />
            <th>created at</th>
          </tr>
          {this.state.list.map((todo, index) => (
            <ToDo key={index} {...todo} />
          ))}
        </table>
      </div>
    );
  }
}
```

위 코드는 예제의 일부분을 가져온 것입니다. 위 코드를 보면 `key` prop에 `index`값을 넣어주고 있는데요 이 경우에 새로운 아이템이 맨앞에 들어올 경우 `key`값 즉 `index`값이 바뀐 채 새롭게 렌더링이 일어나게 됩니다

다음 화면을 보겠습니다

![index-error](images/index-error.gif)

각 아이템에 새로운 렌더링이 일어날 때마다 바뀔 수 있는 `key` 값이 들어가 있기 때문에 기존 `key={0}` 에 들어있는 input의 `value`값이 새로 들어온 아이템에 그려지고 있네요

이 문제는 다음처럼 고유한 `id` 값을 넣어주면 해결 할 수 있습니다

```diff
render() {
    return (
      <div>
        ...
        <table>
          <tr>
            <th>ID</th>
            <th />
            <th>created at</th>
          </tr>
          {this.state.list.map((todo, index) => (
+           <ToDo key={todo.id} {...todo} />
          ))}
        </table>
      </div>
    );
  }
}
```

![normal-key](images/normal.gif)

만약 리스트 항목에 명시적으로 key를 지정하지 않으면 React는 기본적으로 `index`를 key로 사용합니다.

# key를 통해서 렌더링 컨트롤하기

위에서 설명한 `엘리먼트의 타입이 같은 경우` 리액트가 새로운 화면을 그리는 방법에서 벗어나 엘리먼트에 `key` prop을 이용한다면 강제적으로 컴포넌트 인스턴스를 리셋 할 수 있습니다 예상하신 대로 이 방법은 성능 이슈를 유발 할 수 있으니 자주 사용하는 것은 좋은방법이 아닙니다

이에 대한 자세한 예제는 다음 [코드펜](https://codesandbox.io/s/48zl4zoyv0)을 보고 설명 드리겠습니다
<i>(참고: `prevProps` 는 `prevState`로 표시되는 것이 더 정확합니다)</i>

```js
  state = {
    key: true,
    count: 0
  };

  handleChildUnmount = () => {
    this.setState(prevState => ({ count: prevState.count + 1 }));
  };

  toggleKey = () => {
    this.setState(prevState => ({ key: !prevState.key }));
  };

  render() {
    const { key, count } = this.state;

    return (
      <div>
        <button onClick={this.toggleKey}>Toggle Child Key</button>
        <Child key={key} count={count} onUnmount={this.handleChildUnmount} />
      </div>
    );
  }
```

위 예제에서는 `Toggle Child Key` 버튼을 누를때 마다 key값을 true 또는 false로 바꿔 `Child` 컴포넌트가 `unmount` 되어 `handleChildUnmount`가 실행되길 원하고 있습니다. 위 코드펜 예제를 실행했으면 아시겠지만 이는 원하는 대로 동작합니다 심지어 `Child` 컴포넌트에는 다음 처럼 `shouldComponentUpdate`가 false 를 리턴 하지만 컴포넌트 인스턴스가 아예 새롭게 생성되기 떄문에 이는 영향을 미치지 않습니다

```js
  shouldComponentUpdate() {
    return false;
  }
```

위 예제에서 `Child` 컴포넌트에 `key` prop이 없었다면 사실상 `Child`에 변화가 아예 없기 때문에 인스턴스를 새로 그리는 일은 일어나지 않았을 것입니다 이런 `key` prop 속성을 사용한다면 다음 링크 예제의 문제를 해결 할 수 있겠네요(https://kentcdodds.com/blog/understanding-reacts-key-prop)

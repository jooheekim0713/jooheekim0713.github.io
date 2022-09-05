---
title: "[React] PureComponent, memo"
date: 2022-08-31 11:00:00 +/0900
categories: [React]
tags: [React, PureComponent, memo]
---

## React Virtual DOM

- React는 Component에 관련된 state, props이 변경되는 경우 랜더가 호출된다. state, prop이 변경되면 메모리상에 존재하는 Virtual DOM tree에 이전의 prop과 state로 구현된 tree와 새로 구현된 tree를 비교해서 변경된 부분만 DOM에 업데이트한다.
- 문제는 변경되지 않은 Component도 랜더링되기도 하는데 변경사항 없는 요소를 다시 읽어오는 것은 메모리를 낭비하며 성능을 저하시킨다. 따라서 이런 변경이 필요하지 않은 Component들의 랜더링을 막기 위한 성능 최적화가 필요하다.

## React 성능 최적화

- React 성능 최적화를 위해서는 class를 사용할때는 Component를 PureComponent로, 함수형일때는 함수를 memo로 감싸주는 방법이 있다.
- 두 방법 모두 내부 로직에서 props 와 state가 변하지 않는 상황에서 불필요한 랜더링을 막아서 앱의 성능을 향상시킨다는 장점이 있다.
- 하지만 PureComponent와 memo를 분별없이 사용할 경우 오히려 성능이 저하될 수 있기때문에 주의가 필요하다.

### React Developer Tools 설치

- 컴포넌트의 랜더링은 React Developer Tools로 확인할 수 있다. 이 확장 프로그램은 React 앱일 경우에만 개발자 도구 탭에 보인다.

  1. Chrome 웹 확장 프로그램 > [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=ko) 다운
  2. Components 탭 설정(⚙) 팝업에서 highlight updates when components render를 체크하면 component가 랜더될때마다 랜더되는 component들이 하이라이트된다.
     ![react developer tools ](/assets/img/react_developer_tool_render.png)

  3. 이제 랜더링될 필요가 없는 component 들을 확인해 성능 최적화해준다.

### [PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent)

- class가 Component를 extend할때 성능 최적화를 위해 적용할 수 있는 방법이다. PureComponent는 shouldComponentUpdate()함수를 이용해 prop, state를 (이전의 Virtual DOM tree와의) 얕은 비교로 어떤 요소를 업데이트해 DOM에 전달할지 판단한다.

- [PureComponent 사용시 주의사항](https://reactjs.org/docs/render-props.html#be-careful-when-using-render-props-with-reactpurecomponent) : PureComponent 는 shouldComponentUpdate()으로 prop의 변화 여부를 확인하는데 만약 전해지는 prop이 매번 변할 경우 shouldComponentUpdate()을 쓸데없이 불러오게되어 매번 체크하게되어 오히려 성능이 떨어지게 된다.

```javascript
class Mouse extends React.PureComponent {
  // Same implementation as above...
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>

        {/*
          매번 랜더링할 때마다 Mouse class에 전달하는 prop이 변경된다면 
          Mouse class가 PureComponent를 extend하지 말아야한다.
        */}
        <Mouse render={(mouse) => <Cat mouse={mouse} />} />
      </div>
    );
  }
}
```

### [memo](https://ko.reactjs.org/docs/react-api.html#reactmemo)

- 공식문서에는 오직 성능 최적화를 위해 사용되기 때문에 랜더링을 방지하기 위해 사용하지 말라는 경고 메시지가 있다. 랜더링을 막기 위해 무분별하게 사용하지 말라는 의미 같다.
- memo 를 사용해야할 때는 [Use React.memo() wisely](https://dmitripavlutin.com/use-react-memo-wisely/)
  - 컴포넌트가 같은 값이 주어지면 항상 같은 결과가 나올 때
  - 컴포넌트 랜더링이 자주 될 때
  - 항상 같은 props로 랜더링 될 때
  - 컴포넌트가 커서 랜더링시 시간이 오래 걸릴 때

```javascript

const Card = memo(({ card }) => {
  ...
  return (
    ...
  );
});
```

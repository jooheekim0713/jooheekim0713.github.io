---
title: "[React] memo, useRef, useHistory"
date: 2022-08-22 15:15:00 +/0900
categories: [React]
tags: [React, memo, useRef, useHistory]
---

## [React memo](https://ko.reactjs.org/docs/react-api.html#reactmemo)

- 컴포넌트가 동일한 props로 동일한 결과를 렌더링한다면 React.memo를 호출하고 결과를 메모이징하도록 래핑해 결우에 따라 성능 향상을 누릴 수 있다. 마지막으로 랜더링된 결과를 재사용한다. 쓸데없는 랜더링을 줄여준다는 뜻. useState, useReducer, useContext 등의 훅을 사용해 props가 변할 때마다 랜더링을 하는데 (같은 props로 랜더링 될 때) 랜더링이 필요가 없는 component의 랜더링을 방지하기 위해 사용한다.
- 공식문서에는 오직 성능 최적화를 위해 사용되기 때문에 랜더링을 방지하기 위해 사용하지 말라는 경고 메시지가 있다. 랜더링을 막기 위해 무분별하게 사용하지 말라는 의미 같다.
- memo 를 사용해야할 때는 [Use React.memo() wisely](https://dmitripavlutin.com/use-react-memo-wisely/)
  - 컴포넌트가 같은 값이 주어지면 항상 같은 결과가 나올 때
  - 컴포넌트 랜더링이 자주 될 때
  - 항상 같은 props로 랜더링 될 때
  - 컴포넌트가 커서 랜더링시 시간이 오래 걸릴 때

## React useRef

## [useHistory](https://v5.reactrouter.com/web/api/Hooks/usehistory)

- 최신버전인 react-router ver.6 는 useHistory()를 더이상 지원하지 않는다. 따라서 useNavigate()를 사용해보려고 했으나 공식 사이트 useNavigate 항목에 404가 떠서 급 방향을 틀었다. 공식 문서에 404가 뜨는데 섵부르게 사용할 순 없다는 생각이 들었다.
- 2022-08-28 현재는 docs는 여전히 404, hooks 분류에는 정보가 업데이트 되어있다. [React-router useNavigate() 공식 문서](https://reactrouter.com/en/main/hooks/use-navigate)
  - 공식 문서를 보면 이렇게 사용한다. useHistory()와 마찬가지로 첫 arg는 이동하는주소를 입력받고 두번째 arg는 선택입력사항으로 replace 또는 state를 이동하는 주소에 보낼 수 있다.
  - <code> { replace: true } </code>를 사용한다면 페이지가 이동된 후 뒤로가기를 하더라도 기존 페이지로 넘어오지 않고 메인 페이지('/')로 돌아간다. <code>{ replace: false }</code> 인 경우 뒤로가기가 가능하며 replace 속성을 명시하지 않으면 false가 기본값이다.

```javascript
import { useNavigate } from "react-router-dom";

function SignupForm() {
  let navigate = useNavigate();

  async function handleSubmit(event) {
    event.preventDefault();
    await submitForm(event.target);
    navigate("../success", { replace: true });
  }

  return <form onSubmit={handleSubmit}>{/* ... */}</form>;
}
```

- useHistory 훅은 이렇게 사용한다. push 함수를 사용해 주소와 state를 전달 할 수 있다.

```javascript
const Maker = ({ authService }) => {
  const history = useHistory();

  const onLogout = () => {
    authService.logout();
  };

  useEffect(() => {
    authService.onAuthChange((user) => {
      if (!user) {
        history.push("/");
      }
    });
  });

  return(... )
};
```

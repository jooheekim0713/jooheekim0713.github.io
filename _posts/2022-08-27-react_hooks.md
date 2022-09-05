---
title: "[React] useRef, useHistory"
date: 2022-08-22 15:15:00 +/0900
categories: [React]
tags: [React, useRef, useHistory]
---

## [useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)

- 함수형 컴포넌트에서 ref를 사용할 때는 useRef hook을 사용한다.(클래스형 컴포넌트에서는 React.createRef를 사용한다.)
- useRef()를 사용하여 Ref 객체를 만들고, 선택하고 싶은 DOM 에 ref 값으로 설정한다. Ref 객체의 .current 프로퍼티는 선택한 DOM 의 값을 뜻한다.
- useRef는 내용이 변경될 때 변경된 값을 알려주지 않기때문에 React가 DOM 노드에 ref를 attach하거나 detach할 때 어떤 코드를 실행하고 싶다면 대신 콜백 ref를 사용해야한다.

```javascript
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

## [useHistory](https://v5.reactrouter.com/web/api/Hooks/usehistory)

- react의 hook이 아닌 react-router-dom hook이다.
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

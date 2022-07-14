---
title: "[React] 오류 수정"
date: 2022-07-14 12:29:00 +/0900
categories: [React]
tags: [React, React JS, JS, Javascript, strictmode]
---

1. React.StrictMode 설정시 데이터가 두번 불러와지는 현상, 그 이유<br>

   [React StrictMode 관련 공식 문서](https://ko.reactjs.org/docs/strict-mode.html)
   : strict mode의 특징

   - 안전하지 않은 생명주기 메서드를 사용하는 모든 클래스 컴포넌트 목록을 정리해 컴포넌트 관련 경고 로그 출력
   - 이전의 React에서 레거시 문자열 ref API를 사용할때 경고 출력 ( ref API 를 대체해 콜백형태 사용 권장 )
   - 이전에 사용됐지만 현재 권장되지 않는 findDOMNode 사용 경고
   - 권장되지 않는 레거시 contect API 사용 경고
   - **렌더링 단계 생명주기 메서드는 여러번 호출될 수 있기 떄문에 부작용을 포함하지 않는 것이 중요 -> 이중으로 호출해 문제가 되는 부분을 발견하게 도와줌**
     [stackoverflow react 두번 랜더링되는 문제 질문과 답변](https://stackoverflow.com/questions/61254372/my-react-component-is-rendering-twice-because-of-strict-mode)

     -> 해결방법은 간단하다 react 버전을 내리거나 strictmode를 주석처리하거나

2. React.StrictMode 설정시 React-router-dom Link hook이 안 먹는 현상, 그 이유

   - react-router-dom 버전이 react 18버전과 맞지 않아서 발생했던 문제, react-router-dom을 최신 버전으로 올리거나 react버전을 내리면 해결된다.

3. React 'react-dom'과 'react-dom/client'의 차이<br>

   [ReactDOMClient 관련 공식 문서](https://reactjs.org/docs/react-dom-client.html)

   - react-dom/client 패키지는 클라이언트가 app을 열때 동작하는 클라이언트를 위한 메서드
   - 대부분의 component에 적용할 필요가 없다.
   - index.tsx 에 ReactDOM.render()가 아니라 ReactDOM.createRoot()를 사용하는 이유가 이거였음
   - 새로나온 react 18부터 적용됨

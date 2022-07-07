---
title: "[React] React JS styled-components"
date: 2022-07-05 13:53:00 +/0900
categories: [React]
tags: [React, React JS, JS, JavaScript]    
---

 퇴사하기 전에 React 강의를 일단 듣고 따라 치면서 공부했던 기억이 있는데 한동안 취준, 이력서 작성에만 집중하다보니 다 잊어버렸다.<br>
 예전 깃 계정으로 코드를 따라쳤던 기억은 있는데 기록을 안해놔서 어떻게 구현했는지 기억이 하나도 안나요...<br>
 기록의 중요성을 다시 한번 느낀다. 리액트 강의 재수강하면서 어떻게 포트폴리오에 적용할지 생각해보자.<br>
 
 
  - react app 세팅 문서 [react 세팅 공식문서](https://ko.reactjs.org/docs/create-a-new-react-app.html#create-react-app)
 
## styled-components 적용

	npm i styled-components

	
- styled.(HTML component)로 짜여져있기 때문에 component 속성을 바꾸려면 styled뒤의 html component를 바꾼다.<br>
![react div component](/assets/img/react_styled_div.png)
![react h1 component](/assets/img/react_styled_h1.png)
- 공통된 css 속성은 component로 설정하고 공통되지 않은 css 속성은 개별 props로 받는다. <br>
![공통된 속성을 묶어 component로 설정하고 속성을 추가해 공통되지않은 속성을 반영한다.](/assets/img/react_in_props.png)
![class명이 다른 component](/assets/img/react_developer_tool.png)
props가 다르기때문에 클래스명이 같지 않은 것을 확인할 수 있다.
- styled function을 활용해 기존의 속성을 그대로 상속받고 새로운 속성을 추가할 수 있다.(기존 속성에서 props로 받는 데이터가 있다면 props도 잊지않고 추가해야함) <br>
![styled function을 활용해 기존 속성을 상속받음](/assets/img/react_styled_func.png)
- component를 변경하고 싶을때는 as="변경하고싶은 html component명" 을 추가한다.<br>
![component 속성 변경 방법](/assets/img/react_styled_as.png)
- 공통된 component의 속성을 설정할때는 attrs({ 속성 })을 작성한다.<br>
![공통된 attrs 설정](/assets/img/react_common_attrs.png)
- 애니메이션을 구현하기 위해서 styled-component에서 keyframes를 추가하고 from - to 또는 0% - 50% - 100%의 단계으로 애니메이션을 적용할 수 있다.<br>
![keyframes로 애니메이션 구현](/assets/img/react_keyframes_animation.png)
- 컴포넌트 내부의 html component를 조작하기 <br>
![내부의 html component 조작하기](/assets/img/react_inside_component.png)


- 화면 테마 적용하기 ThemeProvider import
 1. index.tsx에서 App 컴포넌트를 Themeprovider 컴포넌트로 감싸고 lighttheme, darktheme에 테마별 변경요소와 색상을 설정한다.
![index.tsx에서 theme적용](/assets/img/themeprovider_index.png)
 2. app.tsx 에서 theme별 props를 받아 배경 색상, 글씨 색상에 반영한다.
![app.tsx에서 theme 속성을 받아서 테마별 색상 변경](/assets/img/themeprovider_app.png)


	
	
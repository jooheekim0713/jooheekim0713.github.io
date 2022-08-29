---
title: "[React] useState callback"
date: 2022-08-29 21:40:00 +/0900
categories: [React]
tags: [React, useState]
---

## useState callback

```javascript
const createOrUpdate = (card) => {
  const updated = { ...cards };
  updated[card.id] = card;
  setCards(updated);
};
```

- 기존의 코드는 이렇게 작성되어 있었다. 문제는 원하는 대로 실행되지 않았다는 것이다.
- 코드 작성의도는 수정된 card의 정보들이 update 변수에 담겨 setCards()함수로 card 의 상태를 업데이트 하는것이 었으나 수정된 사항이 card에 반영이 안됐다. 수정하기 전 state가 자꾸 불러져왔다.
- 이유는 setCards()함수가 비동기적으로 동작하기 때문이다. 따라서 상태를 동기적으로 업데이트하기 위해서는 useState에서 제공하는 callback을 사용하면 된다.
- [Reactjs setState() 비동기 관련 문서](https://ko.reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous) 에 따르면 이렇게 바꿀 수 있다.

![setState 비동기 함수 사용법](/assets/img/setState_async.png)

- 따라서 setCards()내부에서 이전의 cards를 받아서 state를 수정해서 업데이트한 정보를 return한다.

```javascript
const createOrUpdate = (card) => {
  setCards((cards) => {
    const updated = { ...cards };
    updated[card.id] = card;
    return updated;
  });
};
```

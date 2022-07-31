---
title: "[Recoil] atom, selector"
date: 2022-07-29 21:10:00 +/0900
categories: [Recoil]
tags: [Recoil, selector, atom, React]
---

<style>
    p{
        text-indent : 1em;
        margin-top : 20px;
        margin-bottom : 20px;
    }
    .container {
        position: relative;
        padding-bottom: 56.25%; /* 16:9 */
        height: 0;
    }
    .container iframe {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
    span{
         position: relative;
    }
</style>

<strong>
    <a href="https://recoiljs.org/"> Recoil 공식 문서</a>
</strong>
<p>
    Recoil은 React를 위한 state 관리 라이브러리이다.<br/>
    React에서 state는 컴포넌트의 상태고 컴포넌트의 상태가 변경될 때마다 React는 화면을 리랜더링해서 바뀐 정보를 노출시킨다.문제는 React의 기본 state만 사용하면 전역으로 사용하는 상태를 관리가 힘들다는 것이다. 여러 화면에서 공통으로 state에 접근하기 위해서는 컴포넌트 줄줄이 state를 물려줘야한다. 그렇게하면 코드도 또 줄줄이 길어지고 state를 사용하지 않은 페이지에도 state를 물려주게 되어 쓸데없는 코드가 늘어난다. 그리고 이때 등장하는게 Recoil이다.
</p>
<p>
    Recoil은 state가 이리저리 돌아다니는 현상을 막아준다. Recoil은 React 의 state를 atom으로 관리한다. atom을 사용하면 state가 사용되지 않는 컴포넌트를 타고 돌아다니는게 아니라 state정보가 필요한 컴포넌트가 atom에 직접 연결해서 바로 정보를 얻을 수 있다.
</p>
<label for="recoil-youtube">Recoil atom에 대한 youtube 영상</label>
<div class="container" name="recoil-youtube">
    <iframe 
        width="560" height="315" src="https://www.youtube.com/embed/_ISAA_Jt9kI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
    </iframe>
</div>

```
  npm install recoil
```

<p>
  Recoil을 설치한다.
</p>

### Atom

#### App.tsx

```typescript
import React from "react";
import { RecoilRoot } from "recoil";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
  <React.StrictMode>
    <RecoilRoot>
      <App />
    </RecoilRoot>
  </React.StrictMode>,
  document.getElementById("root")
);
```

<p>
  Recoil을 사용하기 위해 RecoilRoot를 import하고 App을 감싼다.
</p>

#### atom.tsx

```typescript
export interface IToDo {
  text: string;
  id: number;
  category: Categories;
}

export const categoryState = atom<Categories>({
  key: "category",
  default: Categories.TO_DO,
});

export const toDoState = atom<IToDo[]>({
  key: "toDo",
  default: [],
});
```

<p>
  atom을 관리하기 위한 atom.tsx를 만들고 사용할 atom과 selector를 작성한다.<br/> 
  atom.tsx 외부에서도 사용할거니까 export 하는건 필수
</p>

#### ToDo.tsx

```typescript
import { useSetRecoilState } from 'recoil';

function ToDo({ text, id, category }: IToDo) {
  const setToDos = useSetRecoilState(toDoState);
  const onClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    ...
    setToDos((oldToDos) => {
      ...
    });
  };
  return (
    ...
        <button name={Categories.TO_DO} onClick={onClick}>
          TO_DO
        </button>
    ...
  );
}

```

<p>
  atom.tsx에 있는 toDoState를 사용하기 위해서는 useSetRecoilState를 import한다.<br/>
  참고로 Recoil atom은 React state와 유사하게 변수와 그 변수를 수정하는 함수 세트를 가지고 있다.<br/>
  useRecoilValue는 atom의 상태를 받아오는 함수고 useSetRecoilState는 atom 상태를 변경하기 위한 함수다.<br/>
  두 기능을 모두 사용하고 싶다면 useRecoilState. useRecoilState 는 useState처럼 atom과 atom을 수정하는 함수를 함께 사용할수 있다.
  ToDo.tsx에서는 setToDos를 사용해서 onClick하면 onClick 함수에서 todo 상태를 변경하는 로직을 작성한다.
</p>

### Selector

<p>
  selector는 atom 혹은 다른 selector의 상태를 입력받아서 동적인 데이터를 반환하는 함수다. 쉽게 말하면 atom, selector의 state 를 받아와서 원하는대로 가공하고 가공된 state를 반환하는 함수다.
</p>

#### atom.tsx

```typescript
export interface IToDo {
  text: string;
  id: number;
  category: Categories;
}

export const toDoState = atom<IToDo[]>({
  key: "toDo",
  default: [],
});

export const categoryState = atom<Categories>({
  key: "category",
  default: Categories.TO_DO,
});

export const toDoSelector = selector({
  key: "toDoSelector",
  get: ({ get }) => {
    const toDos = get(toDoState);
    const category = get(categoryState);
    return toDos.filter((toDo) => toDo.category === category);
  },
});
```

<p>
  toDoSelector는 toDoState와 categoryState 두 atom의 값을 가져오고 원하는 대로 가공해서 화면에 가공된 atom값을 보여준다.
</p>

```typescript
import React from "react";
import { useRecoilState, useRecoilValue } from "recoil";
import { Categories, categoryState, toDoSelector, toDoState } from "../atom";
import CreateToDo from "./CreateToDo";
import ToDo from "./ToDo";

function ToDoList() {
  const toDos = useRecoilValue(toDoSelector);
  const [category, setCategory] = useRecoilState(categoryState);
  const onInput = (event: React.FormEvent<HTMLSelectElement>) => {
    setCategory(event.currentTarget.value as any);
  };
  return (
    <div>
      <h1>To Dos</h1>
      <select value={category} onInput={onInput}>
        <option value={Categories.TO_DO}>TO_DO</option>
        <option value={Categories.DOING}>DOING</option>
        <option value={Categories.DONE}>DONE</option>
      </select>
      <CreateToDo />
      {toDos?.map((toDo) => (
        <ToDo key={toDo.id} {...toDo} />
      ))}
    </div>
  );
}
```

<p>
  selector역시 atom을 사용하는 것과 마찬가지로 useRecoilState, useRecoilValue, useSetRecoilState 함수를 사용해 atom값을 가져오거나, 수정할 수 있다.
</p>

### 🎈참고할만한 블로그

- [[Recoil] 전역 상태관리 라이브러리 - Recoil 정복기](https://abangpa1ace.tistory.com/212)

---
title: "[React] React-hook-form"
date: 2022-07-24 15:00:00 +/0900
categories: [CSS]
tags: [React, React JS, JS, Javascript, React-hook-form]
---

<style>
    p{
        text-indent : 1em;
        margin-top : 20px;
        margin-bottom : 20px;
    }
</style>

<strong>
    <a href="https://react-hook-form.com/"> React Hook Form 공식 문서</a>
</strong>
<p>
    메인 화면에 써 있다 시피 (Performant, flexible and extensible forms with easy-to-use validation.)
    html form 태그를 쉽고 유연하게 사용하도록 도와주는 React hook이다.<br/>
    그중 가장 기능이 많고 이번 프로젝트에 많이 사용한건
</p>

## useForm

<p>
    useForm은 register, watch, handleSubmit, setValue, getValue등의 함수를 제공한다.
</p>

### register, handleSubmit 함수 사용 예시

- input 입력창, select 요소 을 이용해 등록할 떄 사용하는 함수
- handleSubmit 함수는 form validation이 완료되면 다른 함수를 parameter로 받아서 form data를 받을 수 있다.
- onValid로 받은 form 데이터는 객체로 리턴된다.
- html에서 required 속성을 추가해서 form validation할 수 있지만 웹 개발자도구 화면에서 required를 지울 수 있다. javascript로 form에 validation을 추가하면 속성을 지울수 없다는게 장점이다.

```javascript
function ToDoList() {
  const { register, watch, handleSubmit } = useForm();
  const onValid = (data: any) => {
    console.log(data);
  };

  return (
    <div>
      <form onSubmit={handleSubmit(onValid)}>
        <input {...register("Email"), { required: true }} placeholder="Email" />
        <input {...register("lastName")} placeholder="lastName" />
        <input {...register("firstName")} placeholder="firstName" />
        <button type="submit">Add</button>
      </form>
    </div>
  );
}
```

### 공식사이트에 올라온 register 함수의 사용법 예시

- onChange, onBlur 등의 기능을 사용할 수 있다.

```javascript
const { onChange, onBlur, name, ref } = register('firstName');
// include type check against field path with the name you have supplied.

<input
  onChange={onChange} // assign onChange event
  onBlur={onBlur} // assign onBlur event
  name={name} // assign name prop
  ref={ref} // assign ref prop
/>
// same as above
<input {...register('firstName')} />

```

### formState 사용 예시

- 전체 form state에 대한 정보를 갖고있는 객체. 사용자와 form의 interaction을 확인할 수 있다.
- fromState의 errors는 form의 required, pattern에 맞지 않는input이 제출됐을 경우 오류가 뜬 input box를 error message와 함께 보여주는 객체다.

```javascript
function ToDoList() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();
  const onValid = (data: any) => {
    console.log(data);
  };
  console.log(errors);

  return (
    <div>
      <form
        style={% raw %}{{ display: "flex", flexDirection: "column" }}{% endraw %}
        onSubmit={handleSubmit(onValid)}
      >
        <input
          {...register("email", {
            required: "Email is required",
            pattern: {
              value: /^[A-Za-z0-9._%+-]+@naver\.com$/,
              message: "only naver id can be enrolled",
            },
          })}
          placeholder="Email"
        />
        <span>{errors?.email?.message}</span>
        <input
          {...register("lastName", { required: true })}
          placeholder="lastName"
        />

        <span>{errors?.lastName?.message}</span>
        <input
          {...register("firstName", { required: true })}
          placeholder="firstName"
        />

        <input
          {...register("password", {
            required: "Your password is required",
            maxLength: {
              value: 10,
              message: "Your password is too short",
            },
          })}
          placeholder="Password"
        />

        <span>{errors?.password?.message}</span>
        <input
          {...register("password1", {
            required: "password is required",
            minLength: {
              value: 5,
              message: "your password need more than 5 letters",
            },
          })}
          placeholder="Password1"
        />

        <span>{errors?.password1?.message}</span>
        <button type="submit">Add</button>
      </form>
    </div>
  );
}
```

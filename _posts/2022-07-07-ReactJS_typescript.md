---
title: "[React] React typescript"
date: 2022-07-07 09:00:00 +/0900
categories: [React]
tags: [React, React JS, JS, Javascript, TS, Typescript]    
---

	npm install --save typescript @types/node @types/react @types/react-dom @types/jest
	
 - 기존 Create React App 프로젝트에 타입스크립트 설정 추가
	[React JS 타입스크립트 공식문서](https://create-react-app.dev/docs/adding-typescript/)
 - TS 설정을 추가 + 파일 확장자명을 .tsx 후 발생한 오류 경로를 찾지 못하고있다. 
	![](/assets/img/react_ts_compile_error.png)
	1. .tsx 를 붙이면 화면은 제대로 나오지만 import path가 .tsx로 끝날수 없다고 경고창이 뜬다.
	![typescript ](/assets/img/react_ts_path_error.png)
	2. ~~//@ts-ignore을 App import 위에 붙여서 임시방편으로 오류를 해결함~~ 
	
	3. 아무래도 ts 설정이 제대로 되지않은것 같아서 tsconfig.json파일을 추가해줌.
	공식문서에는 tsconfig.json파일이 알아서 생성되니까 따로 만들 필요가 없다고 했으나~ 안 만들어지니까 일단 만들어봄
	![tsconfig.json 관련 공식문서](/assets/img/react_ts_handbook.png)
	![tsconfig.json파일을 추가해준다. ](/assets/img/react_ts_tsconfig.png)
	
	```json
	{
	  "compilerOptions": {
		"target": "es5",
		"lib": ["dom", "dom.iterable", "esnext"],
		"allowJs": true,
		"skipLibCheck": true,
		"esModuleInterop": true,
		"allowSyntheticDefaultImports": true,
		"strict": true,
		"forceConsistentCasingInFileNames": true,
		"noFallthroughCasesInSwitch": true,
		"module": "esnext",
		"moduleResolution": "node",
		"resolveJsonModule": true,
		"isolatedModules": true,
		"noEmit": true,
		"jsx": "react-jsx"
	  },
	  "include": ["src"]
	}

	```
	해결 ^^ㅎ....
	
	참고) 설정파일이 엉켰다는 생각이 들때는 node_modules, package-lock.json파일을 지우고 다시 설치하자.
	[설정파일 삭제 및 설치 관련 블로그](https://bobbyhadz.com/blog/react-module-not-found-cant-resolve-styled-components)
	```shell
	# 👇️ delete node_modules and package-lock.json
	rm -rf node_modules
	rm -f package-lock.json

	# 👇️ clean npm cache
	npm cache clean --force

	npm install
	```
	
 - JS에서 사용하던 라이브러리나 패키지(A)를 TS에서도 사용하고 싶다면  npm @types/(A) 로 구현되어있는지 확인하고 설치하자.
	그래도 없다면 [DefinitelyTyped 문서](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types)에 있는지 확인해보고 직접 만들어서 DefinitelyTyped에 기여해보기... 언젠간!
	
 - 널 병합 연산자 (nullish coalescing operator ??) : 왼쪽 value가 null이거나 undefined일때 ?? 오른쪽의 value를 반환한다.
	
	```typescript
	const foo = null ?? 'default string';
	//'default string'
	```
	
	![TS의 새 Operator ??](/assets/img/ts_nullish_operator.png)
	
 - defaulttheme 설정을위해 declaration file을 생성한다.
   [declaration file 설정 관련 공식 문서](https://styled-components.com/docs/api#create-a-declarations-file)

	1. DefaultTheme에 원하는 theme 요소와 그에 해당하는 타입을 설정한다.<br>
	![styled.d.ts 파일 생성](/assets/img/react_styled.d.ts.png)
	
	2. DeafultTheme을 받는 theme.ts파일을 생성하고 각 요소에 해당하는 색상을 설정한다.<br>
	![lightTheme, darkTheme 요소와 그에 해당하는 색상 설정](/assets/img/react_theme.ts.png)
	
	3. index.tsx에 ThemeProvider 를 import하고 theme을 설정한다. <br>
	![index.tsx ThemeProvider에 원하는 theme 압력](/assets/img/react_index.tsx.png)
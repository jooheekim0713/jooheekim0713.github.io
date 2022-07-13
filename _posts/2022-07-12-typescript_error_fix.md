---
title: "[Typescript] 오류 수정"
date: 2022-07-08 15:43:00 +/0900
categories: [Typescript]
tags: [Typescript, TS, JS, JavaScript]    
---

1. No overload matches this call
	
	ApexChart를 적용할때 발생한 오류다.
	
	![타입스크립트 no overload matches this call 오류 화면](/assets/img/no_overload_matches.png)

	[참고한 블로그](https://velog.io/@dosilv/TypeScript-React에서-TypeScript-적용하기-오류-해결하기)
	
	
	간단하게 말하면 overloaded function에서 지정한 매개변수들의 타입 형식(여기서는 [number, number] 또는 [string, string])과 실제 전달한 인자의 타입 형식이 일치하지 않으면 뜨는 에러이다.
	
	**따라서 interface를 생성해 IChartAxis[]로 데이터 타입을 지정해준다.**
	
	```typescript
	
	interface IChartAxis {
	  x: Date;
	  y: number[];
	}

	...	
	
	
	function chart(){
		
		...
		return{
			
			...
	
	            {
              name: 'Price',
              data: data?.map((props) => {
                return {
                  x: new Date(props.time_open * 1000),
                  y: [
                    parseFloat(props.open),
                    parseFloat(props.high),
                    parseFloat(props.low),
                    parseFloat(props.close),
                  ],
                };
              }) as IChartAxis[],
            },
          ]}
	```
	
	[ApexChart candlestick 타입 설정 공식문서](https://apexcharts.com/react-chart-demos/candlestick-charts/basic/)
---
title: JavaScript
category: cs
author: "이정훈"
tags: [javascript]
img : https://user-images.githubusercontent.com/114923190/209463375-3894baf5-f616-4f80-8c9a-590ecf8ef6bc.png
comments_disable: true
meta_description: "javascript"
---

## JavaScript 란? 

JavaScript (이하 JS) 이전은 HTML / CSS도 아주 간단한 스타일만 적용 
가능한 정적인(static service) 웹 생태계였다.

문서를 웹으로 볼 수 있는 수준으로 이는 신문활자를 그냥 웹에서 보여주는
수준이라고한다.

<img src = 'https://user-images.githubusercontent.com/114923190/209463209-27a8de49-e484-49fd-b32c-d778ae2d5dde.png' width = '400'>

## 자바스크립트의 탄생비화

JS는 [정적(Static) > 동적(Dynamic)] 으로 표현 할 수 있게 만들어 졌는데
시초는 Natscape라는 웹 브라우저를 통해 상호작용을 강조하기 위해 간단한 기능을 넣어 
만든것이 JS의 시초였다.

웹 개발자들은 각자의 브라우져 생태계에 맞추어 모든 코드를 별도로 작성해야 했고 이를 위해 표준화 작업을 시행, 지금의 [ECMAScript (ECMA-262)]를 표준으로 제정하기 시작하였다. 
지금 우리가 사용하는 JS는 Ecma International 에서 제정 된 표준이 구현된것으로 볼 수 있다.

## ES6란
	- JavaScript는 위에서 언급했듯 Netscape에서 개발하였다.
	- 각자의 브라우저에 맞는 언어를 개발하는 것이 부담되어 하나로 통합표준화 
	     > ECMAScript < 
	- 매년 꾸준히 변경 2021 기준 ES12 까지 나와있다.
	- ES6(ECMAScript 2015)는 6번째 버전으로 2015에 나와서 
	     정말 많은 기능이 변함

### ES5 > ES6
	- ES6 이후를 Morden JavaScript 라고 부름
	- ES5(2009) > ES6(2015)는 약 6년간의 시간 차이가 있음
	- 대표적인 10가지 정도만 알아보자
	1. let, const 키워드 추가 : 암묵적 재할당이 가능해져 호이스팅에 
	       변화를 가지고 옴
	2. Arrow function 추가 : 함수의 간소화
	3. Defult parameter 추가 : 기존에는 기존매개변수의 초깃값을 
	      설정 하려면 함수 내부에서 로직이 필요하였음
	4. Template literal 추가 : `백틱`, ${JS 표현식} 이 가능해짐
	5. Multi-line string : '+', '\n' > `백틱` 으로 줄바꿈이 간편
	6. Class : 객체 생성 방식의 도입
	7. Module : 재사용을 위한 코드의 생성 (export, import)
	8. Distructuring 할당 : 비구조화를 통해 Obj의 사용이 편리해짐
	9. Promise : 'Callback📞 Hell🔥'의 에러처리를 효과적으로 할 수 있게 되었음
	10. String method : include, startsWith, endsWith 검사 로직을 수행할 수 있게 됨 

#### 왜? ES12가 나온 시점에 ES5 || 6 를 알아야 하는가?
	모든 회사가 최신 문법을 사용하는 것은 아니다, 최신문법을 사용하더라도
	이것 또한 시간이 지나면 Legercy(유산)Code가 되어 유지보수가
	필요 할 수 있다.
### Java🍖 !== JavaScript🐹

Java와 JS는 이름이 비슷해 비슷한 언어로 오해를 받지만 이는 개발 당시 
Java의 인기에 편승한 네이밍에 불가하다. !== 전혀 관계가 없다..

<img src = 'https://user-images.githubusercontent.com/114923190/209463375-3894baf5-f616-4f80-8c9a-590ecf8ef6bc.png' width = '400' >


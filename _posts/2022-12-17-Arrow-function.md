---
layout: post
title: JavaScript Arrow function
subtitle: JavaScript 화살표 함수
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Language
 - Programming
banner:
 image : /assets/images/posts/arrow.png
---

화살표 함수는 ECMAScript 6(ES6)에서 도입된 JavaScript에서 함수 표현식을 생성하기 위한 간결하고 보다 현대적인 구문입니다. 화살표 함수는 더 짧은 구문을 제공하며 기존 함수 표현식과 비교하여 동작에 약간의 차이가 있습니다.

화살표 함수의 구문은 다음과 같습니다.

```javascript
(parameters) => expression
```


다음은 화살표 함수의 간단한 예입니다.
```javascript
const add = (a, b) => a + b;  
console.log(add(1, 2)); // Output: 3`
```


이 예에서 'add'는 두 개의 매개변수 'a'와 'b'를 사용하고 그 합계를 반환하는 화살표 함수입니다. 
함수 본문이 단일 표현식으로 구성된 경우 중괄호나 `return` 키워드가 필요하지 않습니다. 
더 복잡한 논리 또는 여러 명령문의 경우 중괄호와 `return` 키워드를 사용할 수 있습니다.
```javascript
const multiply = (a, b) => {   
const result = a * b;   
	return result; 
};  
console.log(multiply(2, 3)); // Output: 6
```



화살표 함수는 기존 함수 표현식과 비교하여 동작에 몇 가지 차이점이 있습니다.

1.  어휘 `this`: 화살표 함수에는 자체 `this` 값이 없습니다. 
2. 대신 둘러싸는 범위에서 `this` 값을 상속합니다. 
3. 이는 `.bind()`를 사용하거나 원하는 `this` 값을 저장하기 위해 별도의 변수를 생성할 필요가 없기 때문에 화살표 함수를 콜백 또는 이벤트 핸들러로 사용할 때 유용할 수 있습니다.

예:
```javascript
function Timer() {   
	this.seconds = 0;    
	
	setInterval(() => {     
		this.seconds++;     
		console.log(this.seconds);   
	}, 1000); 
}  
	const timer = new Timer(); // Output: 1, 2, 3, ...
```



2.  'arguments' 개체 없음: 화살표 함수에는 기존 함수에 있는 특수 'arguments' 개체가 없습니다. 그러나 여전히 나머지 매개변수 구문을 사용하여 가변 개수의 인수를 캡처할 수 있습니다.

예:
```javascript
const sum = (...args) => args.reduce((total, num) => total + num, 0);  
console.log(sum(1, 2, 3, 4)); // Output: 10
```

3.  `prototype` 없음: 화살표 함수에는 `prototype` 속성이 없으며 생성자로 사용할 수 없습니다.
4.  `yield` 없음: 화살표 함수는 생성기 함수로 사용할 수 없으며 화살표 함수 내에서 `yield` 키워드를 사용할 수 없습니다.

요약하면 화살표 함수는 JavaScript에서 함수 표현식을 생성하기 위한 간결하고 현대적인 구문입니다. 더 짧은 구문을 제공하고 어휘 'this'가 있으며 'arguments' 객체, 'prototype' 및 'yield'와 같은 기존 함수 표현식의 일부 속성과 동작이 부족합니다. 화살표 함수는 콜백으로 또는 함수형 프로그래밍 패턴으로 작업할 때 특히 유용할 수 있습니다.
---
layout: post
title: javascript 배열 'Array'
subtitle: 자바스크립트의 Array
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Programming
 - Language
banner: 
 image : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
---

## 배열 (array)

-   같은 타입의 테이터들을 하나의 변수에 할당하여 관리하기 위해 사용하는 데이터 타입

### 배열의 선언

-   new Array(  ) / `[ ]`

```javascript
const arr1 = new Array(1,2,3,4,5)
const arr2 = [1,2,3,4,5]
```

### 배열 안의 데이터

-   배열에 있는 데이터 각각을 '요소(element)'라 부름
-   배열에서는 '인덱스(index)'가 객체의 속성명과 같은 역할
-   index는 배열 안 데이터의 순서 (0 부터 시작)

```javascript
const rainbowColors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']
console.log(rainbowColors[0])		//red
console.log(rainbowColors[1])		//orange
console.log(rainbowColors[2])		//yellow
console.log(rainbowColors[3])		//green
console.log(rainbowColors[4])		//blue
console.log(rainbowColors[5])		//indigo
console.log(rainbowColors[6])		//violet
```

### 배열의 길이

-   length

```javascript
const rainbowColors = ['red', 'orange', 'yellow', 'green', 'blue', 
					   'indigo', 'violet']

console.log(rainbowColors.length)						// 7	
console.log(rainbowColors [rainbowColors.length - 1])	// Violet
```

### 배열의 요소 추가와 삭제

-   push, pop

```javascript
const rainbowColors = ['red', 'orange', 'yellow', 'green', 'blue', indigo', 'violet']
                       

rainbowColors.push('ultraviolet')		// 마지막 요소 'ultraviolet'
console.log(rainbowColors)

rainbowColors.pop()						// 마지막요소 'violet'
console.log(rainbowColors)
```

### 배열과 반복문

-   배열의 요소들을 하나씩 꺼내어 출력

```javascript
const rainbowColors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']

for (let i = 0; i < rainbowColors.length; i++) {
    console.log(rainbowColors[i])
}
```

1.  배열의 인덱스는 0부터 시작하므로 변수 i의 시작값도 0 (let i = 0;)
2.  i가 배열의 길이보다 작을 떄에만 반복문 안의 코드 실행  
    ( i < rainbowColors.length;)
3.  모든 요소를 출력해야하므로 i는 1씩 증가 ( i++ )

-   좀 더 간단한 형식의 for 문

```javascript
const rainbowColors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']

for (const color of rainbowColors) {
    console.log(color)
}
```

1.  배열에서 요소들을 차례대로 하나씩 찾아 color라는 변수에 할당
2.  자동으로 배열의 끝까지 반복문 실행

-   상품가격 데이터 배열 합계와 평균 구하기

```javascript
const priceList = [1000, 2000, 4000, 50000, 15000, 10000, 22000, 38000, 10000, 4000]
let sum = 0
for (const price of priceList) {
    sum += price					// assingment Operators (대입연산자)
}

const avg = sum / priceList.length	// sum을 배열의 길이만큼 나눈다.
console.log(`합계: ${sum}, 평균: ${avg}`)
```
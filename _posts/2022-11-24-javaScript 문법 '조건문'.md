---
layout: post
title: javaScript 문법 '조건문'
subtitle: 자바스크립트이 조건문
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Programming
 - Lanaguage
banner:
 image : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
---

## 조건문

### if

-   boolean 값
-   if 구문을 만족했을 때만 코드를 실행
-   if (조건) {조건을 만족할 때 실행할 코드}

```javascript
const shoesPrice = 40000
if (shoesPrice < 50000) {
	console.log('신발을 사겠습니다.')
	} 	// 신발가격이 '50000' 아래이므로 console에 '신발을 사겠습니다' 출력

const capPrice = 60000
if (capPrice < 50000) {
	console.log('모자를 사겠습니다.')
	}	// 모자 가격이 50000원 이상이므로 console에 출력 안됨
```

### else, else if

-   if 구문의 조건을 만족하지 않았을때 실행하고 싶은 코드를 else구문과 작성

```javascript
const shoesPrice = 60000
if (shoesPrice < 50000) {
	console.log('신발을 사겠습니다.')
	} else {
    	console.log('너무 비싸요. 신발을 사지 않겠습니다.')
    }
    	# console 에 else 값 출력
```

-   else if

```javascript
const shoesPrice = 50000
if (shoesPrice < 50000) {
	console.log('신발을 사겠습니다.')
	} else if (shoesPrice <=50000) {
    	console.log('고민해 볼게요')
    } else {
    	console.log('너무 비싸요 사지 않겠습니다')
    }
    
    	# console 에 else if 값  출력
```
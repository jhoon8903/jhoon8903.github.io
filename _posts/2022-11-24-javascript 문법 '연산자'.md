---
title: javascript 문법 '연산자'
category: javascript
author: "이정훈"
tags: [javascript]
img : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
comments_disable: true
meta_description: "javascript 문법 '연산자'"
---

## 연산자

### +를 사용하여 문자열을 붙일 수 있다.

```javascript
consolr.log('My' + ' car')			# My car
console.log('1' + 2)				# 12
```

### 템플릿 리터럴 (template Literals)

-   백틱(``)을 사용, 문자열데이터를 간결히 표현 가능.

```javascript
const shoesPrice = 20000
console.log(`이 신발의 가격은 ${shoesPrice}원 입니다.`)
= console.log('이 신발의 가격은 ' + 'shoesPrice' + '원 입니다.')
# 이 신발의 가격은 20000원 입니다.
// + 를 활용한 붙이기보다 간결하게 표현 가능
```

### 산술연산자 (numeric Operators)

-   사칙연산 외 나머지연산 거듭제곱 등

```javascript
console.log(2 + 1)		# 3
console.log(2 - 1)		# 1
console.log(4 / 2)		# 2
console.log(2 * 3)		# 6
console.log(10 % 3)		# 1 //remainder 나머지 연산자
console.log(10 ** 2)	# 100 // exponentiation 거듭제곡 10의 2승
```

### 증감연산자 (increment and decrement Operators)

-   자기 자신의 값을 증가시키거나 감소시키는 연산자  
    변수앞 / 뒤에 놓느냐에 따라 차이 발생

```javascript
let count = 1
const preIncrement = ++count
// 먼저 자기 자신에게 1을 더해서 변수에 재할당 후 , 이를 preIncrement에 할당한다.
// count = count + 1
   const preIncrement = count

console.log(`count: $(count}, preIncrement: ${preIncrement}`)
# count: 2, preIncrement: 2
```

```javascript
let count = 1
const postIncrement = count++
// postIncrement에 자기 자신의 값을 먼저 할당 후, 1을 더해서 재할당
// const postincrement = count
   count = count + 1
console.log(`count: ${count}, postIncrement: ${postIncrement}`)
# count: 2, postIncrement: 1
```

### 대입연산자 (assignment Operators)

-   어떤 값을 어떤 변수에 할당한다 = 대입연산자를 사용한다.  
    =, +=, -= 등 연산과 대입을 한번에 가능

```javascript
const shirtsPrice = 100000
const pantsPrice = 80000
let totalPrice = 0

totalPrice += shirtsPrice 	# tP = tP + sP  / 100000 = 0 + 100000 
totalPrice += pantsPrice	# tP = tP + pP / 180000 = 100000 + 80000
totalPrice -= shirtPrice	# tP = tP - sP / 80000 = 180000 - 100000
```

### 비교연산자 (comparison Operators)

-   True, False > boolean 연산자

```javascript
console.log(1 < 2)		# True
console.log(2 <= 2)		# True
console.log(1 > 2)		# False
console.log(1 >= 2)		# False
```

### 논리연산자 (logical Operators)

-   or, and, not 등의 연산자  
    || (or)는 연산 대상 중 하나만 True여도 return  
    && (and)는 연산 대상이 모두 True 여야만 trturn  
    ! (not)는 True를 False로 False를 True로 바꿔서 return

```javascript
let isOnSale = True
let isDiscountItem = True

console.log(isOnSale && isDiscountItem)		# True and True 이므로 True
console.log(isOnSalr || isDiscountItem)		# True or True 이므로 True

isOnsale = False
console.log(isOnSale && isDiscountItem)		# False and True 이므로 False
console.log(isOnSalr || isDiscountItem)		# False or True 이므로 True

inDiscountItem = False
console.log(isOnSalr && isDiscountItem)		# False and False 이므로 False
console.log(isOnSalr || isDiscountItem)		# False or Fasle 이므로 False

console.log(!isOnSalr)						# not False 이므로 True
```

### 일치연산자

-   두 값이 일치하는지 비교

```javascript
console.log(1 === 1)		# True
console.log(1 === 2)		# False
console.log('Javascript' === 'Javascript')		# True
console.log('Javascript === 'javascript')		# False

// === 와 == 의 차이
console.log('Javascript == 'javascript')		# True
console.log(1 === '1') 							# False
console.log(1 == '1')							# True
```
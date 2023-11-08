---
layout: post
title: javaScript 'Class, Object'
subtitle: 자바스크립트 객체지향
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Programming
 - Language
banner: 
 image : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
---

## Class와 Object

-  물리적으로 존재하거나 추상적으로 생각할 수 있는 것중에서 자신의 속성을 갖고 있고 다른 것과 
  식별가능한 것

### Class 선언

```javascript
class Notebook {                            // 키워드와 class명
    constructor(name, price, company) {     // 생성자
        this.name = name                    // this와 속성(property)
        this.price = price
        this.company = company
    }
}
```

-   class 키워드와 class명  
    class 선언 후 class명이 나옴 [class:class 선언],[Notebook:키워드]
-   생성자 (constructor)  
    객체가'생성'될 때 javascript 내부에서 호출되는 함수
-   this와 속성(property)  
    this는 class를 사용해 만들어지는 객체 자기 자신을 의미  
    뒤에 붙는 name,price,company 는 객체속성  
    = 뒤에 붙는name,price,company 는 매개변수

### Object 만들기

-   const 변수명 = new class명(생성자에서 정의한 매개변수)

```javascript
const notebook1 = new Notebook('MacBook',2000000,'Apple')
```

-   new 라는 키워드 먼저 써주고 class명을 함수처럼 호출
-   객체의 속성 하나하나에 접근해 데이터를 갖고와야 할 때 this.속성명 사용

```javascript
const notebook1 = new Notebook('MacBook',2000000,'Apple')

console.log(notebook1)
console.log(notebook1.name)			// Macbook
console.log(notebook1.price)		// 2000000
console.log(notebook1.company)		// Apple
```

### 메소드 (method)

-   Class에는 데이터(값)를 나타내는 속성 뿐만 아니라 함수와 같이 특정 코드를 실행할 수 있는 method도 정의할 수 있다.

```javascript
// Class 선언
class Product {
    constructor(name, price) {
        this.name = name
        this.price = price
    }


    printInfo() {
        console.log(`상품명:${this.name}, 가격:${this.price}원`)
    }
}

// 객체 생성 및 method 호출
const notebook = new Product ('Apple Macbook', 2000000)
notebook.printInfo()

> 상품명:Apple Macbook, 가격:2000000원
```

### 객체 리터럴 (object Literal)

-   object Literal 은 class와 같으 템플릿없이 빠르게 객체를 만들 수 있는 방법
-   2개 이상의 method, property가 있을때는 ' , ' 로 구별

```javascript
const computer = {
    name: 'Apple Macbook',      // property
    price: 20000,               // property
    
    printInfo: function() {     // method
        console.log(`상품명: ${this.name}, 가격: ${this.price}`)
    }
}
computer.printInfo()

> 상품명: Apple Macbook, 가격: 20000
```
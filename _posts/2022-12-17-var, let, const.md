---
layout: post
title: var, let, const
subtitle: JavaScript의 변수
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Language
 - Programming
banner:
 image : /assets/images/posts/ffCqyCc.png
---

변수 선언
--
## 선언 (declaration) => 초기화 (initialization)

## var

![](https://i.imgur.com/ffCqyCc.png)

var : 함수 범위, 변수가 정의된 함수와 모든 중첩 함수 내에서 액세스 가능, 함수 외부에서 선언되면 var 변수는 전역 범위를 갖음, 호이스팅 됨
        코드 실행 전에 해당 범위의 맨 위로 이동되고 undefined 값으로 초기화 됨, 변수가 선언된 줄 전에 변수에 액세스 하려고하면 예기치 않은 동작이 발생할 수 있음
```javascript
function exampleVar() {
  console.log(myVar); // Output: undefined
  var myVar = 'Hello';
  console.log(myVar); // Output: 'Hello'
}
```


## let

![](https://i.imgur.com/VRfreW8.png)

let : 블록 범위, 변수가 정의된 블록 내에서만 액세스할 수 있음, 의도한 범위 밖에서 실수로 변수에 액세스하거나 수정하는 것을 방지할 수 있음
        var와 달리 let 변수는 호이스팅되지 않으므로 선언 전에 let 변수에 액세스하려고 하면 ReferenceError가 발생
```javascript
function exampleLet() {
  console.log(myLet); // Output: ReferenceError
  let myLet = 'Hello';
  console.log(myLet); // Output: 'Hello'
}

```

## const

![](https://i.imgur.com/FFmnRZj.png)

const : let과 유사하게 const로 선언된 변수는 블록 범위이며 호이스팅되지 않음, const 변수는 선언 시 값을 할당해야 하며 초기화 후에는 해당 값을 변경할 수 없다는 것
             수정되어서는 안 되는 상수나 값을 선언하는 데 적합, 다만 객체는 할당 가능
```javascript
function exampleConst() {
  console.log(myConst); // Output: ReferenceError
  const myConst = 'Hello';
  console.log(myConst); // Output: 'Hello'
  myConst = 'World'; // Output: TypeError (Assignment to constant variable)
}
```


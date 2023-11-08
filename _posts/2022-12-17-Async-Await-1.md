---
layout: post
title: Async-Await-1
subtitle: 비동기 함수 Async Await
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Programming
 - Language
banner:
 image: /assets/images/posts/Yopc9NI.jpg
---

원문 : https://dev.to/lydiahallie/javascript-visualized-promises-async-await-5gke#syntax

Async / Await
--

## Async / Await 실행 예문

```javascript
const one = () => Promise.resolve('One!')

async function myFunc() {
	console.log('In function!')
	const res = await one()
	console.log(res)
}

console.log('Before function!')
myFunc();
console.log('After function')
```

![](https://res.cloudinary.com/practicaldev/image/fetch/s--aOWmZxnV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/e5duygomitj9o455107a.gif)

## 1.  console.log('Before function') 먼저 실행 'Call Stack'에서 빠져나감
![](https://res.cloudinary.com/practicaldev/image/fetch/s--bfscMU3t--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/d27d7xxiekczftjyic4b.gif)

## 2. myFunc 함수 호출 > myFunc() >Console.log('In function') 'Call Stack을 빠져나감'
![](https://res.cloudinary.com/practicaldev/image/fetch/s--wN7yFTnt--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/9wqej2269vmntfcuxs9t.gif)

## 3. Await를 만나면 Async 함수는 Queue에 대기 시키고, 로컬에서 빠져나서가 Global 컨텍스트 실행  > Console.log('After function')
![](https://res.cloudinary.com/practicaldev/image/fetch/s--UC78HoCO--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/b6l3psgewvtrtmrr60tg.gif)

## 4. 컨텍스트 전체 실행 시킨 후 JS 엔진이 Queue에 남아 있는 myFunc()를 실행 > console.log(res)
![](https://res.cloudinary.com/practicaldev/image/fetch/s--V8u36kEG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/hlhrtuspjyrstifubdhs.gif)

---
'async/await'는 Promise와 같은 비동기 작업 작업을 단순화하는 JavaScript의 구문 기능입니다. 
비동기 코드를 작성하고 읽기 위한 보다 동기식 구문을 제공하므로 이해하고 유지하기가 더 쉽습니다.

`async`는 비동기임을 나타내기 위해 함수 선언 또는 함수 표현식 앞에 사용하는 키워드입니다. 
`async` 함수는 항상 Promise를 반환합니다. 
함수가 Promise가 아닌 값을 반환하면 JavaScript는 해결된 Promise에서 해당 값을 자동으로 래핑합니다.

![](https://i.imgur.com/Yopc9NI.jpg)



비동기 함수의 예:

```javascript
async function myAsyncFunction() {
  return 'Hello, World!';
}

myAsyncFunction().then((value) => {
  console.log(value); // Output: 'Hello, World!'
});
```

`await`는 `async` 함수 내에서만 사용할 수 있는 키워드입니다. 
적용되는 Promise가 이행되거나 거부될 때까지 `async` 함수의 실행을 일시 중지하는 데 사용됩니다. 
'await' 키워드는 Promise의 해결된 값을 풀기 때문에 보다 동기식으로 보이는 방식으로 비동기 코드를 작성할 수 있습니다.

Await 사용 예 :

```javascript
async function fetchAndProcessData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  console.log(data);
}
```

위의 예에서 `await`는 두 번 사용됩니다. 먼저 `fetch()` Promise가 응답 객체로 해결될 때까지 기다린 다음 `response.json()` Promise가 실제 데이터로 해결될 때까지 기다립니다. 
이렇게 하면 프로미스와 함께 `.then()` 및 `.catch()`를 사용하는 것보다 코드를 더 쉽게 읽고 이해할 수 있습니다.

`await`를 사용하면 전체 JavaScript 이벤트 루프가 차단되지 않고 현재 `async` 함수의 실행만 차단된다는 점에 유의해야 합니다. 
다른 비동기 작업은 백그라운드에서 계속할 수 있습니다.

`async/await`를 사용할 때 `await` 표현식을 `try-catch` 블록으로 래핑하여 거부된 약속을 포착하고 적절하게 처리함으로써 오류를 처리할 수 있습니다.

async/await를 사용한 오류 처리의 예:

```javascript
async function fetchAndProcessData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}
```

요약하면 'async/await'는 JavaScript의 구문 기능으로, 보다 동기식 구문을 제공하여 약속과 같은 비동기 작업 작업을 단순화합니다. 
'async' 키워드는 비동기 함수를 선언하는 데 사용되며 'await' 키워드는 약속이 해결되거나 거부될 때까지 실행을 일시 중지하기 위해 'async' 함수 내에서 사용됩니다. 따라서 비동기 코드를 더 쉽게 읽고 이해하고 유지 관리할 수 있습니다.
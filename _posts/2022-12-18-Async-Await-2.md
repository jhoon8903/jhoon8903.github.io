---
layout: post
title: Async-Await-2
subtitle: 자바스크립트 비동기 함수 2
category: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Language
 - Programming
banner:
 image : /assets/images/posts/async2.png
---

동기 (Synchronous) & 비동기(Asynchronous)
--

- '동기'로 실행된다 = 먼저 실행된 코드의 결과나 나올때까지 '대기'하는 것
- '비동기'로 실행된다 = 실행된 순서와 관계 없이 결과가 나오는 것
<img src = 'https://user-images.githubusercontent.com/114923190/209463982-686944de-9c52-49a8-a356-81a9ea2a416b.png' width='500'>

### Blocking Model && Non-Blocking Model
	- Blocking Model 이란, 코드의 실행이 끝나기 전까지 '실행 제어권을 다른곳에 넘지기 않아 
	    다른 작업을 하지 못하고 대기' 하는것 을 말함
	- Non_Blocking Model이란, 코드의 실행이 끝나지 않아도 '실행 제어권을 다른곳에 넘겨 
	    다음 코드가 실행'될 수 있는 것을 말함.
	    
<img src = 'https://user-images.githubusercontent.com/114923190/209464014-40577bee-d968-47dd-a8b3-ca8f5546cdf0.png' width = '600'>

JS는 Async + Non-Blocking Model을 채용하여 '현재 실행중인 코드의 실행이 끝나지 않아도' 
다음 코드를 '호출' 합니다.

<img src = 'https://user-images.githubusercontent.com/114923190/209464150-f3f3644a-8dd2-4582-9eff-1d4c25383223.png' width = '400'>

JS 는 각 명령어들이 순서대로 실행될 수 있게 구현되어 있지만, Non-Blocking Model에 의해
'동기적 명령' 아닌 '모든 함수는 비동기적으로 실행됩니다.'
#### 동기, 비동기와 뭐가 다른것인가.
	- 제어권을 넘기면 Non-Blocking > 비동기적 처리가 가능
	- 제어권을 넘기지 않으면 Blocking > 비동기적 처리가 '가능한 환경'이어도 비동기적 처리가 
	    불가능하다.
<img src = 'https://user-images.githubusercontent.com/114923190/209464569-ee3754d9-03b3-419b-b6c4-e6e98ddb9360.png' width = '500'>

## 프로미스 (Promise)
 - JS Async를 Sync로 처리할 수 있게 돕는 Built-in (미리 내부적으로 정의된) 객체 유형
 - Promise 객체를 이용하면 Async Process를 손쉽게 할 수 있도록 돕는다.

 #### 비동기 처리를 Why❓ 동기적으로 처리해야하나❓❓❓❓❓
	 - JS관점에서 동기적인것처럼 처리한다는 것
	 - 만약 위 예제에서 First를 먼저 출력하고 싶다면 Middle과 Last는 무조건 기다려야 함
	 - 비동기적 진행 선택은 '개발자의 의도'로 결정 된다.
	 - DB와의 통신은 I/O 이고, JS는 비동기적 I/O 이므로 데이터를 가져오기도 전에 
	     명령이 실행되면 그것은 곧 'ERROR'를 발생시킴

이때 필요한 것이 'Promise' '언제 진행할지 약속'이다.
언제란 '비동기적 명령이 실행이 완료된 이후'를 말하는 것이다.
위의 예를 들어 데이터를 DB에서 가져온 후 데이터를 처리하는 명령어들을 Promise 이후에 
진행 하도록 작성하는 것

### Promise 생성자(constructor) 인터페이스
 - executor에는 함수만 올 수 있으며 '인자'로는 [resolve, reject]가 주입된다.
 - executor는 Promise 실행 함수라고 불리고, Promise가 만들어질 때 자동으로 실행된다.
 - Promise가 연산을 언제 종료하는지 상관하지 않고 '인자'중 하나는 무조건 호출해야 함
 ![Pasted image 20221224022819](https://user-images.githubusercontent.com/114923190/209464633-69422af8-4e0d-42d7-91a6-c28aca737113.png)

#### 생성자 (Constructor)
	JS에서 Primitive를 제외한 대부분의 타입은 객체(OBject)형식
	객체를 생성하는 함수를 생성자(Constructor)라고 부름, promise또한 객체이기 때문에 
	const(생성자함수)로 객체선언
<img src = 'https://user-images.githubusercontent.com/114923190/209467028-fbcb44de-0904-4bf1-b5e6-ed1eaa22a17b.png' width = '500'>


	Promise는 반드시 3가지 상태를 지닌다.
		- Pending : 대기 [이행하거나 거부되지 않은 초기 상태]
		- Fulfilled : 이행 [연산이 성공적으로 완료됨]
		- Reject : 거부 [연산이 실패함]
<img src = 'https://user-images.githubusercontent.com/114923190/209467147-04c07eac-6eaa-4cc9-b74d-e5076a0ee7a2.png' width = '500'>

<img src = 'https://user-images.githubusercontent.com/114923190/209467160-a616fb5c-8a68-4f08-ab2c-b800fbdc4231.png' width = '500'>


#### Promise.then
<img src = 'https://user-images.githubusercontent.com/114923190/209467207-a39b6e34-86e3-42a8-9fe3-f4f9745786ed.png' width = '500'>

#### Promise.catch
<img src = 'https://user-images.githubusercontent.com/114923190/209467223-1e241d52-86e1-42d3-bab0-209a8a8bf122.png' width = '500'>


#### Promise.all
	배열처럼 순회가능한 객체를 매개변수로 한다 'Promise.all(iterable);'
	순회가능한 객체에 주어진 모든 Promise를 이행한 후, 혹은 Promise가 주어지지 않았을 때
	이행하는 Promise를 반환한다. 주어진 Promise 중 reject가 있으면, 첫번째로 reject한
	Promise 이유를 사용해 자신도 거부한다.
[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all]

#### Promise.race
	순회가능한 매개변수 중 가장 먼저 완료된것을 결과값으로 이행한다. 
	'Promise.race(iterable)'
[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/race]


## 비동기 함수 (Async Function)
	특징
		- 비동기 함수는 (=>) 비슷하지만 2가지가 다름
		1. 비동기 함수의 결과 값은 항상 Promise 객체로 resolve 된다.
		2. 비동기 함수안에서만 await 연산자를 사용할 수 있다.
<img src = 'https://user-images.githubusercontent.com/114923190/209467241-8ae9c29b-a76f-4acb-aac3-e29a888a2515.png' width = '500'>

<img src = ' https://user-images.githubusercontent.com/114923190/209467340-c0380915-5196-41d9-b47f-ada2a04673a6.png' width = '500'>

#### 비동기 함수를 쓰는 이유
	- await 연산자를 비동기 함수 안에서만 사용가능한데,
		이를 활용하면 문법이 훨씬 간결해질 수 있음
	- new Promise(executor) 코드로 Promise를 직접 생성하면 'executor'가
	   바로 실행 되는 것 과는 달리, '비동기함수'는 함수가 실행되기 전까지 'Promise'를 생성안함

#### Await 연산자
	- await 연산자를 사용하면, Promise - Fulfilled 상태가 되거나, reject 될 때 까지
	   함수의 실행을 중단하고 기다릴 수 있다.
	- Promise의 연산이 끝나면 함수에서 반환한 값을 얻을 수 있다.
	- await 연사자는 Asyns 비동기 함수 안에서만 사용 가능

```JavaScript
const result = await 값;
```

<img src = 'https://user-images.githubusercontent.com/114923190/209467349-5b0034e5-11f2-460f-b129-a3c8bf7dec77.png' width = '500'>


동기식 및 비동기식은 특히 프로그래밍 및 컴퓨팅 컨텍스트에서 프로세스, 기능 또는 작업이 시스템에서 실행되는 방식을 설명하는 데 사용되는 용어입니다. 이러한 개념은 시스템의 서로 다른 구성 요소 간의 코드 실행 및 통신을 구성하는 방법을 이해하는 데 중요합니다.

1.  동기식:

동기식 실행에서는 프로세스, 기능 또는 작업이 차례로 순차적으로 수행됩니다. 동기식 프로세스는 다음 프로세스가 시작되기 전에 완료되어야 합니다. 이 접근 방식은 실행 흐름이 선형적이고 예측 가능하기 때문에 이해하고 구현하기가 더 쉽습니다.

그러나 동기식 실행은 특히 네트워크 요청이나 파일 I/O 작업과 같이 시간이 많이 걸리는 작업을 처리할 때 성능 문제를 유발할 수도 있습니다. 이러한 경우 전체 시스템 또는 응용 프로그램이 차단되어 느린 프로세스가 완료될 때까지 기다리면 사용자 경험이 저하되거나 리소스가 비효율적으로 사용될 수 있습니다.

동기 작업의 예로는 디스크에서 파일 읽기 또는 시스템이 다음 작업을 진행하기 전에 결과를 기다리는 데이터베이스 쿼리 만들기가 있습니다.

2.  비동기식:

비동기 실행에서 프로세스, 기능 또는 작업은 다른 작업이 완료될 때까지 기다리지 않고 동시에 또는 서로 독립적으로 실행될 수 있습니다. 비동기 실행은 시스템이 느리거나 예측할 수 없는 작업이 완료되기를 기다리는 동안 다른 작업을 계속 처리할 수 있으므로 시간이 많이 걸리거나 예측할 수 없는 작업을 처리할 때 특히 유용합니다.

비동기식 실행은 느리거나 시간 소모적인 작업으로 인해 시스템이 차단되는 것을 방지하므로 리소스를 보다 효율적으로 사용하고 전반적인 성능을 향상시킬 수 있습니다. 그러나 비동기 작업의 완료를 처리하고 결과를 관리하려면 콜백, 약속 또는 async/await와 같은 고급 프로그래밍 기술이 필요한 경우가 많기 때문에 구현 및 관리가 더 어려울 수도 있습니다.

비동기 작업의 예로는 시스템이 서버의 응답을 기다리는 동안 다른 작업을 계속 처리할 수 있는 HTTP 요청 만들기 또는 파일을 읽는 동안 시스템이 다른 코드를 계속 실행할 수 있는 디스크에서 큰 파일 읽기가 있습니다.

요약하면 동기식 및 비동기식은 프로세스, 기능 또는 작업이 시스템에서 실행되는 방식을 설명합니다. 동기식 실행은 작업을 차례대로 순차적으로 수행하는 반면, 비동기식 실행은 작업을 동시에 또는 서로 독립적으로 실행할 수 있습니다. 비동기식 실행은 더 나은 성능과 리소스 활용으로 이어질 수 있지만 작업 완료와 그 결과를 관리하기 위해 더 복잡한 프로그래밍 기술이 필요할 수 있습니다.
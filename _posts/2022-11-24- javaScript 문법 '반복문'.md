---
layout: post
title: javaScript 문법 '반복문'
subtitle: 자바스크립트 Loop
categories: JavaScript
author: Daniel
tags: 
 - JavaScript
 - Language
 - Programming
banner:
 image : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
---

## 반복문

### while

-   반복문을 활용하여, 특정코드 반복실행 가능
-   while (조건) {조건을 만족할 때 실행 코드}

```null
let temperature = 20
while (temperature < 25) {
	console.log(`{trmperature}도 정도면 적당한 온도입니다.`)
    temperature++  // 증감연산자 활용 온도증가변화
    }
    * 주의 할 점 반복문에 포함된 변수의 값을 계속 변화시켜주어 
      언젠가는 반복문이 끝날 수 있게 해주어야 함.
      
      반복문이 계속 True값을 return 한다면 무한루프에 빠짐
      이럴 떄는 ctrl + c 를 눌러 강제 종료
```

### for

-   while과 같은 반복문
-   for (begin; condition; step) {조건을 만족할 떄 실행할 코드}

```null
for (let temperature = 20; temperature < 25; temperature++) {
		console.log(`${temperature}도 정도면 적당한 온도입니다.`)
    }
    
    // 20 ~ 24도 까지 증가 후 코드 종료
    // 변수(이하 temp)를 선언하고 값을 할당 (begin;)
       temp값 연산. 결과값이 True라면 계속, False면 종료 (condition;)
       중괄호 안에 코드 실행, temp값에 증감연산 +1을 더하여 반복 (step)
```

### 반복문과 조건문 활용

-   1 ~ 10 까지 숫자중 3으로 나누어 떨어지는지 출력

```null
for (let number = 1; number < 10; number++) {
    if (number % 3 === 0) {
    	console.log(`${number}는 3으로 나누어 떨어지는 숫자입니다`)
    } else {
        console.log(`${number}는 3으로 나누어 떨어지지 않는 숫자입니다.`)
    }
}
```

-   1 ~ 20 까지 숫자 중 홀, 짝 구별 출력

```null
for (let number = 1; number <= 20; number++) {
    if (number % 2 === 0){
        console.log(`${number}는 짝수 입니다.`)
    } else {
        console.log(`${number}는 홀수 입니다.`)
    }
}
```
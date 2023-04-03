---
title: javaScript 함수
category: javascript
author: "이정훈"
tags: [javascript]
img : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
comments_disable: true
meta_description: "javaScript 함수"
---

## 함수

-   함수란 특정 작업을 수행하는 코드의 '집합'

```javascript
const priceA = 1000
const pricaB = 2000
const sum1 = priceA + pricaB
console.log(`두 상품의 함계는 ${sum1}입니다.`) // 합
const avg1 = sum1 / 2
console.log(`두 상품의 평균 금액은 ${avg1}입니다.`) // 평균 

// 위의 코드가 반복 된다면 사용 해야 하는게 함수
```

### 함수의 선언과 호출

-   함수의 선언
-   function 함수명_prameter (매개변수들) {  
    이 함수에서 실행할 코드  
    return 반환값  
    }

```javascript
 function calculateAvg(price1, price2) {
    const sum = price1 + price2
    console.log(`두 상품의 합계는 ${sum}입니다.`)
    const avg = sum / 2
    return avg
}
```

-   함수의 호출

```javascript
function calculateAvg(price1, price2) {
    const sum = price1 + price2
    console.log(`두 상품의 합계는 ${sum}입니다.`)
    const avg = sum / 2
    return avg
}

const priceA = 1000
const priceB = 2000

const avg1 = calculateAvg(priceA,priceB)
console.log(`두 상품의 평균은 ${avg1}입니다.`)

const priceC = 3000
const priceD = 4000

const avg2 = calculateAvg(priceC, priceD)
console.log(`두 상품의 평균운 ${avg2}입니다.`)
```
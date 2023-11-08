---
title: JavaScript의 자료형과 JavaScript만의 특성은 무엇일까?
category: javascript
author: "이정훈"
tags: [javascript]
img : https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTab05l3ndGtZqyqxgTeOkmB7g2eDGyYrQp60gRu108tIEXOLQTl8tf9Jpx90UiNJEIv1Q&usqp=CAU
comments_disable: true
meta_description: "JavaScript의 자료형과 JavaScript만의 특성은 무엇일까?"
---

미니프로젝트를 진행하고 코드를 짜보면서 기초는 때고 다음단계로 넘어가겠구나 했는데 이게 왠걸,,  
javaScript책을 보고 강의를 들으면서 나는 진짜 JS에 1도 모르고 이썼구나 싶더라,,,

그냥 겉핥기식으로만 공부하는건 나중에 크게 문제가 될 것 같아 좀 더 깊게 파고들어야겠다.

# JavaScript의 자료형과 JavaScript만의 특성은 무엇일까?

## Loosely Typed의 Dynamic 언어

### 느슨한 타입 이란 ( What is th loosely Typed ? )

느슨한 타입은 [타입 없이 변수 선언] 하는 것, < > 강력한 타입 (strong typed)

> -   javaScript loosely typed  
>     var a = 13; // number 선언  
>     var b = "thirteen"; // string 선언

> -   javaScript strongly typed  
>     int a = 13; // int 선언  
>     String b = "thirteen"; // string 선언

-   var로 선언했다고 하여 type이 아닌건 아닌것도 아니고 var 타입 인것도 아니다, 다만 strong type 보다 유연하게 변수를 선언 할 수 있는대신 code가 길어지고, 복잡해지면 오류가 발생 할 수 있는 빈도가 더 높아질 수 있다.
    
-   javaScript는 원시타입은 총 5가지 ( Number, Strng, Boolean, Null, Undefined )!== var
    
-   지금은 var 타입으로 사용을 지양 하고있다.
```javascript
> var a = 'Macbook_pro14'  
> console.log(a) ==> Macbook_pro14  
> var a = = 'Macbook_pro16'  
> console.log(a) ==> Macbook_pro16
```

이와 같이 var 은 선언하는 변수가 동일하게 중복되어도 Error를 띄우지 않으므로 코드가 단순할때는 그 오류를 어렵지 않게 찾을 수 있겠지만, 코드가 길어지고 데이터량이 많아지기 시작하여 중복되는 변수들이 발생하기 시작하면 그 오류를 잡기위해 큰 리소스들이 소모되므로 지금에 와서 var은 사용을 지양한다.

-   가끔 Google에서 코드를 긁어올때 보이는 var () 들은 대부분 게시글의 게시 시간이 좀 경과한 경우가 많기 때문에 이러한 것이 반영이 안될 수 있으므로 혹시 다른 코드를 copy한다면 특별한 주의가 필요하다.
    
-   그래서 사용하는 것이 let 과 const이다.
```javascript
    > let a = 'Macbook_Air13'  
    > console.log(a) ==> Macbook_Air13  
    > let a = 'Macbook_pro13'  
    > console.log(a) ==> Uncaught SyntaxError: Identifier 'a' has already been declared  
    > a = "LG_Gram_15"  
    > console.log(a) ==> LG_Gram_15
```    

    [ var ()과 다르게 재선언에 대한 오류가 발생한다. ]  
    하지만 let 은 변수를 위와 같은 형태로 재선언이 가능하다.
    
    하지만 상수 const는 재할당마져 불가능 하기 때문에 중복에 대한 에러등을 금방 찾을 수가 있다.
    

### JavaScript의 형변화 ( Type Coercion )

-   javaScript는 느슨한타입으로 관리되기 때문에 내부적으로 타입변환되기가 쉽다.  
    이는 Number + 'String' 이 합쳐지는 경우등에서 볼 수 있다.
```javascript 
7 + 7 + '7' = 147 (Num + Str의 조합으로 Num > Str 으로 형변화를 일으킴
```

-   비교연산자의 경우에도 형변화를 일으킬 수 있다.
```javascript
> 1 == true; > true  
> 1 === true; > False  
> 4 == '4' > true  
> 4 === '4' > False
```

-   미니프로젝트 당시 db에 들어간 가입자의 나이가 Num방식이 아닌 Str 방식으로 return되어 나오는 경우가 많이 있었다. 이것 또한 형변화에 의한 에러 발생 요인이므로 사전에 int() 를 추가하여 Num type으로 변화시켜 데이터를 처리하는 방법 또한 유의하여야 한다.
    
-   비교연산시 Error 를 줄이려면 느슨한 Javascript에서는 (=== 일치연산자를 사용해야 오류를 줄일 수 있다.)
    

### Loosely Typed와 Dynamic 언어의 문제점과 보완방법

-   느슨한 타입과 동젝 언어의 문제점은 그 변수를 다룸에 있어 재할당, 재선언에 있어 타입이 변하거나 변수가 변하여 맨처음 데이터를(변수)를 잃어버릴 수 있다는데에 문제가 있다(type error의 발생), 이를 보완하기 위해서는 `(===)`비교연산자 또는 const 등 상수표현등 원본 데이터를 지킬 수 있는 방법을 생각하면서 코드를 작성해야하며, 아직 다루지 못한 TypeScipt / Flow[타입검사기] 등을 사용 할 수 있다.
    
    > 타입체커에 대해서는 해당 링크에서 다루고 있다.  
    > [https://blog.rhostem.com/posts/2017-06-11-adopting-flow-and-typescript](https://blog.rhostem.com/posts/2017-06-11-adopting-flow-and-typescript)
    

### undefined와 null의 차이 비교

공통점  
1. 둘 다 JS의 원시(Primitive)이다.  
2. 둘 다 변수에 값이 없음을 나타낸다.

-   undefined : 변수를 할당하고나서 변수를 [입력하지 않은 상태]
-   null : 변수를 할당함에 있어 [ " "빈 값을 할당한 상태]
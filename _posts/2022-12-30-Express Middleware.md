---
title: Express Middleware
category: javascript
author: "이정훈"
tags: [node, javascript]
img : https://i.imgur.com/0FQEQuL.png
comments_disable: true
meta_description: "Express Middleware"
---

# Express의 Middleware

express 자체가 자체적으로 최소한 Routing 및 Middleware 기능을 갖춘 WEB Framework 
req, res 이후 일반적으로 next라는 변수로 표시

각 미들웨어를 모듈화하면 각 각의 기능을 효율적으로  재사용 가능


## 종류 
 - application middleware
 - router middleware
 - error handling middleware
 - basic middleware
 - third-party middleware

### Application middleware
- 어플리케이션 전역에서 처리 가능 - app에 대한 req발생할 때 마다 실행
- app.use( ) 또는 app.method ( get, post, put 등 소문자 http 메소드)  
![](https://i.imgur.com/CT2YU0Y.png)

### Router middleware
- router 단위로 req 발생하면 실행
![](https://i.imgur.com/ukGyyXp.png)


### Error Handling middleware
- 4가지의 인수 (err, req, res, next )를 가지고 있으며, 시스템 에서는 인수 숫자를 바탕으로 
   error handling middleware 구분함 (next가 필요없어도 넣어야한다)
```javascript
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```


### Basic middleware 
- static 정적자원을 제공 ( CSS, HTML 등)
- express.static(root,[option]) 형식을 갖추고 있다.
```javascript
express.static(root,('option'))
```
대표적 express middleware
- body-parser / cookie-parser 등
#### option
- dotfiles : dotfile을 제공하기 위한 옵션 [allow, deny, ignore(default) ]
- etag : etag의 생성 설정 [true(default) . false]
- extensions : 파일확장자 설정 [array, 'index.html(default)']
- index : 인덱스 파일전송 [true(default) , false]
- lastModified : 파일이 마지막 수정된 날짜를 헤더에 설정 [true(default) , false]
- maxAge : milesecond 또는 ms형식의 문자열 Cache-Control의 특성을 설정 [ 0 (default) ]
- redirect : 경로 이름이 directory 인 경우 ' / ' 로 경로 재지정 [true (default)  , false] 
- setHeaders : 파일을 제공하도록 HTTP 헤더 설정 [ function (default)  ]

### Third - party middleware
- 추가적인 기능을 제공 별도의 설치 필요
- passport / join-io  등
- 기능이 좋아 적극적으로 사용하면 개발속도나 안정성에서 유리


# Middleware 구동

![](https://i.imgur.com/0FQEQuL.png)

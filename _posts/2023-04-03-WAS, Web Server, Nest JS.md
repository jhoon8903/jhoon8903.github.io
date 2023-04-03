---
title: WAS, Web Server, Nest JS 
category: cs
author: "이정훈"
tags: [cs, nest js, node js]
img : https://www.temok.com/blog/wp-content/uploads/2022/04/What-is-the-Difference-between-Web-Server-vs-Application-Server-1-2.jpg
comments_disable: true
meta_description: "WAS, Web Server, Node JS "
---

# 결론 '상황에 따라 변하는 정보를 제공할 수 있는가?'

![](https://www.temok.com/blog/wp-content/uploads/2022/04/What-is-a-Web-Server.jpg)
[TEMOK](https://www.temok.com/blog/web-server-vs-application-server/)

***

# Web Server ?

- 웹 서버는 [HTTP Protocol](https://jhoon8903.github.io/posts/2022-12-18-Http)을 처리합니다.
- HTTP 요청이 Web Server(이하 WS)에서 우신될 때마다 HTTP 응답을 반환합니다.
- WS는 요청에 대한 응답으로 정적(Static) HTTP page 또는 image를 제공합니다.

![](https://i.imgur.com/EuF6bkv.png)* Static page의 경우 (WS)에 미리 저장된 파일(HTML, CSS, JS, Image)등 을 불러와 구성하는 페이지
- 요청이 WS로 정송되면 응답 자체를 생성하는 대신 추가 처리를 위해 다른 프로그램
    ( 여기서는 File System Database )으로 리디렉션합니다.
- WS는 요청에 대한 설정된 응답만을 처리할 수 있으며, 응답을 관리하는 기능을 제공하지 않음
- **즉, 서버에 저장 된 데이터가 변경되지 않는 한 사용자는 고정된 웹 페이지를 제공받게 된다.**

## Web Server의 주요기능
- HTTP Protocol을 관리하고, 정적 응답을 생성
- 웹 서버와 관련된 서버 측 프로그래밍이 없음
- servlet, html, php, jsp 등과 같은 웹 어플리케이션을 지원
- EJB(Enterprise JavaBeans)에 대한 지원도 제공하지 않음
- DB Connection pooling에 대한 지원도 제공하지 않음

## Static Web page의 장 • 단점

### 장점
- Request에 대한 데이터만 전송하고 추가적인 작업이 없어 빠르다.
- 비용이 적게 든다.

### 단점
- 저장된 데이터만 보여줄 수 잇어 서비스가 한정적이다.
- 삽입, 수정, 삭제 등의 작업이 모두 수독적이므로 관리가 힘들다.

***

# Web Application Server ?

- WAS는 HTTP와 같은 다양한 Protocol을 사용하는 Business Logic에 대한 
  Client Application Access를 제공
- WS는 웹 브라우져에서 HTTP 응답만 처리하는 반면, WAS는 Business Logic을 
  CSR 기반 Application에 노출함
- Dynamic Information, Data, Method 드의 형태를 취하는 Logic이 포함됨
![](https://i.imgur.com/d4mPFZl.jpg)

## Web Application Server의 주요기능
- Dynamic Business Logic을 제공
- 계산, 데이터 처리 및 저장과 같은 백엔드 기능을 처리할 수 있음
- Application, Security, Dependency Injection, EJB 및 Data Pooling Deploy 가능
- WS 보다 많은 기능을 갖춘 상위 서버

## Dynimic Web page의 장 • 단점

### 장점
- Client Req에 따라 Dynamic page를 생성, 제공 하므로 서비스가 다양해짐
- 삽입, 수정, 삭제 등의 작업 관리가 쉬움

### 단점
- Client에게 Web page 를 구성해주기 전, 처리하는 Business Logic이 있어 상대적으로 느림
- 추가적인 비용 발생

***

# WS 와 WAS의 차이점

|Web Server|Web Application Server|
|:------|:------|
|Client의 HTTP요청을 수행하고 생성된 응답을 보냄|Business Logic을 Client에게 공개하고 Dynamic 응답을 보냄 |
|적은 리소스 사용|많은 리소스 사용|
|HTTP Protocol만 지원|HTTP 및 RPC / RMI Protocol 지원|
|수명이 짧은 자원 집약적 프로세스를 처리하도록 설계|자원 집약적이지 않은 웹 트래픽을 관리하도록 설계|
|웹 브라우저에 표시되는 HTTP 요청에 대한 응답을 생성|애플리케이션과 클라이언트-서버 간에 지속적으로 데이터를 교환|

***

# WS와 WAS를 분리하는 이유

## Web Server가 필요한 이유
 
- 정적인 파일들을 WAS까지 가지 않고 빠르게 처리가 가능
- 정적 컨텐츠만 처리하도록 기능을 분배하여 서버의 부담을 줄일 수 있음

## WAS가 필요한 이유?

- Web Page는 Static / Dynamic 모두 존재
- Client 요청에 맞게 적절한 Contents를 만들어서 제공해야 한다.  
- WS 만을 이용한다면 Client가 요청에 대한 결과값을 모두 미리 만들어 놓고 서비스를 해야 한다. 
- WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어서 제공함으로써 자원을 효율적으로 사용할 수 있다.

## WAS가 Web Server의 기능도 모두 수행하면 되지 않을까?

### 1.  기능을 분리하여 서버 부하 방지
- WAS는 DB 조회나 다양한 Business Logic을 처리하느라 바쁘기 때문에 Static Contents는 
  WS에서 빠르게 Client에 제공하는 것이 좋다.
  
- Static Contents 요청까지 WAS가 처리한다면 Static Contents 처리로 인해 부하가 커지게 되고,
  Dynamic Contents의 처리가 지연됨에 따라 수행 속도가 느려진다.
  
- 페이지 노출 시간이 증가.

### 2.   물리적으로 분리하여 보안 강화
- SSL에 대한 암복호화 처리에 Web Server를 사용
  
### 3.  여러 대의 WAS를 연결 가능
- Load Balancing을 위해서 Web Server를 사용
- fail over(장애 극복), fail back 처리에 유리
- 특히 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 
  무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.
- 예를 들어, 앞 단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.

### 4. 여러 웹 어플리케이션 서비스 가능
- 예를 들어, 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우

### 5.  기타
- 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.

즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성 을 위해 Web Server와 WAS를 분리한다.  
Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 
분산 처리가 가능하다.


***

# Express Js, Nest JS는 WAS 인가?
![](https://i.imgur.com/O8HKbUL.png)

## "자바 진영의 WAS 기능은 엄밀히 말해, Node.js로 구현이 가능"

JS는 본래 브라우저에서만 동작할 수 있는 언어였으나 Node.js를 통해 JS 언어로 브라우저 뿐만 아니라 각종 범용 프로그램을 만들 수 있게 되었습니다. 

Node.js는 JS의 런타임이고 쉽게 말해, JS 실행기라고 이해하시면 될 것 같습니다. 

express는 미니멀 웹 프레임워크이며 NestJS는 express 프레임워크를 보다 더 구조화되고 디자인 패턴에 맞게 사용할 수 있도록 도와주는 express의 메타 프레임워크의 역할을 합니다. 

물론 NestJS 자체만으로 사용할 수도 있고 express와 비슷한 fastify의 메타 프레임워크로서도 사용할 수 있습니다.  

** "자바 진영의 WAS 기능은 엄밀히 말해, Node.js로 구현이 가능합니다. "**

정리하자면, Node.js는 V8 엔진 기반의 JS 런타임(실행기)이며, WAS 기능을 구현할 수 있습니다. 
express는 미니멀 웹 프레임워크이고 NestJS는 웹 프레임워크입니다.
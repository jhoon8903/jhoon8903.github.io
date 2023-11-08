---
layout: post
title: REST API (Resource)
subtitle: URI의 이해와 설명
categories: Network
author: Daniel
tags: 
 - Network
 - CS
banner:
 image : https://i.imgur.com/hCzFxRi.png
---

# Representational State Transfer

URI를 통해 자원을 표시하고, HTTP Method를 이용하여 해당 자원의 행위를 규정하여 
그 결과를 받는것이다 

# 여기서 URI 란?

## URI || URL || URN

![400](https://i.imgur.com/hCzFxRi.png)

URI : 자원의 식별자
URL : 자원을 위치
URN : 자원의 이름

### URI (Uniform Resource Identifier) 통합 자원 식별자
	- 통합 자원 식별자는 인터넷에 있는 자원을 어디에 있는지 '자원 자체를 식별하는 방법'
		+ Unirorm : Resource를 식별하는 통일된 방식
		+ Resource : 자원 / URI로 식별 할 수 있는 모든 것
								   L: 웹브라우저의 파일만이 아닌 실시간 교통정보 등 우리가 구분할 수 있는
									     모든것이 Resource
		+ Identifier : 다른 항목과 구분하는데 필요한 정보
	- URI는 인터넷에서 요구되는 기본조건으로 인터넷 프로토콜에 항상 붙어 다님
		+ 프로토콜 (Protocol) : 컴퓨터간에 원활한 통신을 위해 지키기로 약속한 규약,
												신호처리법, 오류처리, 암호, 인증, 주소 등을 포함한다.
			++ 원활한 통신을 위해선 반드시 프로토콜을 통일시켜야 한다. 
				  그래서 전세계에서 쓰이는 프로토콜을 통합시킨 국제 표준 통신규약이 존재한다. 
				  이 표준 프로토콜은 UN 산하의 ITU라는 기관에서 국제통신규약을 만들어 사용한다.
				  +++ 인터넷 프로토콜 : HTTPS / HTTP / feed / tel / mailto / IPFS
	- 하위개념으로 URN / URL이 있다 (사진 참조)

### URL (Uniform Resource Locator) 파일 식별자
	- 네트워크 상에서 자원이 어디 있는지 위치를 알려주기 위한 규약
	- Computer Network와 검색 메커니즘에서의 위치를 지정하는, Web Resource에 대한 참조
	- 웹 사이트의 주소 뿐만 아니라, Computer Network상의 모든 자원을 나타내는 표기법
	- 해당 주소에 접속하려면, URL에 맞틑 Protocol (http / sftp / smp... 등등등)을
	  알아야하고, 그와 동일한 Protocol로 접속해야한다.

### URN (Uniform Resource Name) 통합 자원이름
	- urn:scheme을 사용하는 URI를 위한 역사적 이름
	   + 스킴(scheme) : 구체적이고 확정 된것 / 자료구조, 리스트를 편하게 쓰기위한 
								     고급프로그래밍 언어 (AI분야에서 많이 쓰임)  !== Schema
	- URL이 위치를 지정한다면, URN은 Resource에 이름을 부여하는 것
	- URN은 영속적이며 위치에 도릭접인 자원을 위한 지시자로 사용하기 위해서 
	    1997년 RFC 2141 문서서 정의됨.
	    + IETF 표준규격 [RFC 2141, RFC 3406] 
	    + URN은 유일성, 영속성ㆍ확 장성ㆍ융통성ㆍ규모성 등을 제공할 수 있어야 한다라는 내용.
	- Resource가 Name에 Mapping 되어 있어야 하기에 거의 찾기가 힘들다.
	   그래서 대부분 URL을 사용

### 각 요소 구분하기

![](https://i.imgur.com/YkhU9Hj.png)

***

# Representational State Transfer

URI를 통해 자원을 표시하고, HTTP Method를 이용하여 해당 자원의 행위를 규정하여 
그 결과를 받는것이다 

구성 
[[자원 Resource]] (자세한내용은 클릭):  URI
행위 Verb : HTTP Method
표현 Representations

# REST 원칙
## Uniform
	- Uniform Interface : URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 
											  인터페이스로 수랭하는 아키텍쳐

## Stateless
	- 무상태성 성격을 갖는다. 작업을 위한 상태 정보를 따로 저장하고 관리하지 않는다.
	- 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 
	    단순히 처리하면 된다. 
	- 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.

## Cacheable
	- TTP라는 기존 웹 표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용
	- HTTP가 가진 캐싱 기능 적용이 가능하다.
	- HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용해 구현할 수 있다.

## Self - descriptiveness
	- REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어 있다.

## Client - Server 구조
	- REST서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보) 등을 
	    직접 관리하는 구조로 각각의 역할을 확실하게 구분한다.
	- 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로 간 의존성이 줄어든다.

## 계층형 구조
	- REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조 
	    상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 
	    있게 한다.

***

# 설계원칙

1. URI 는 정보의  리소스를 표현해야 한다.
		- 행위에 대한 표현이 아닌 리소스를 표현하는데 중점을 두어야 한다.
		- 리소스명은 동사보다 명사를 사용한다.
2. 리소스에 대한 행위는 HTTP Method로 표현한다.
		Method [ Create / Read / Update / Delete / HEAD / OPTIONS / CONNECT / TARCE]  
		method 에 대한 설명 [[HTTP Method]]
3. ( / ) 구분자는 계층 관계를 내포
4. URI의 마지막 문자로 ( / )를 포함하지 않는다.
5. ( - )은 URI의 가독성을 높이는데 사용 할 수 있다.
6. ( _ )는 URI에 사용하지 않는다.
7. URI 경로는 소문자를 사용한다,
		- RFC3986(URI 문법 형식)은 schema 와 Host를 제외하고는 upper와 lower를 
	  구별하도록 규정
			  + IETF 에서 2005년에 발표된 'Uniform Resource Identifier (URI): Generic Syntax' 협약 규정  [[https://www.rfc-editor.org/rfc/rfc3986]]		  
8. 파일확장자는 URI에 포함시키지 아니한다.
		- 대신 Accept header를 사용.
			+ HTTP header
				+ + Content-Type header
						-  HTTP 메시지(req,res)에 담겨 보내는 데이터의 형식을 알려주는 헤더
						-  대부분의 브라우져 및 웹은 Content-Type header를 기준으로 메세지에 담긴
			     데이터를 분석하고, 파싱함
					    -  HTTP req의 GET방식인 경우 무조건 URL 끝에 query string 으로 
			     key = value 로 날아가기 때문에 Content-Type header가 필요 없음
						-  Content - Type는 POST / PUT 처럼 req.body에 실어 보내는 경우에 중요
			      <'form'> tag를 통해 첨부파일 전송의 경우라면 브라우저가 자동으로 
						      > multipart/form-data로 설정하여 req message를 보냄   
				
				+ + Accept header
						- 브라우저에서 웹서버로 req message 에 담기는 header
						- 브라우져가 req message의 Accept header값을 application/json이라고
			     설정하였다면 = '나는 json 데이터만 처리할 수 있으니 json 데이터 형식으로 
			     응답을 돌려줘'라는 것과 같음
						- http://restapi.example.com/members/soccer/345/pthoto.jpg(X)
						   GET /members/soccer/345/photo HTTP/1.1 (O) 
									Host restapi.example.com 
									Accept: image/jpg 
									
## URI 설계 개념

![](https://i.imgur.com/SOuzvLr.png)



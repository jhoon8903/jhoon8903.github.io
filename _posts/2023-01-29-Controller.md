---
title: Controller
category: nestjs
author: "이정훈"
tags: [nestjs]
img : https://i.imgur.com/HGnED1T.png
comments_disable: true
meta_description: "Controller"
---

![](https://i.imgur.com/HGnED1T.png)
[Nest Js Official_Doc]('https://docs.nestjs.com/controllers')

목적 : Application 에 대한 특정 Request를 수신 하는 것
controller의 생성 : 기본 컨트롤러를 만들기 위해서는 'class' && 'decorators' 가 필요함 
							*decorators*는 *class*를 필수 데이터와 연결하고 라우팅맵을 생성할 수 있도록 도와줌

#### Get 요청 Controller 예제
```ts
import {Controller, Get} from '@nestjs/common'

@Controller('cats')
export class CatsController {
	Get()
	findAll() : string {
		return "'.../cats'라는 엔드포인트로 Get요청 methods가 요청오면 이 문자열을 반환한다."
	}
}

//client res : "'.../cats'라는 엔드포인트로 Get요청 methods가 요청오면 이 문자열을 반환한다."
```

표준(권장하는 사용법)
- 기본 제공 메소드 사용시 자동JSON 형태로 직렬화 (serialized) , 'boolean' 타입은 직렬화를 하지 않고 반환 
- @HttpCode(...) 201을 사용하는 메소드를 제외하고는 기본적으로 항상 status 200 을 반환

라이브러리별
- 라이브러리에서 제공하는 (ex. Express) 메소드 사용시 해당하는 응답처리 메소드 사용가능
```ts
@Res() 
findAll(@Res() response)
	response.status(200).send( )
```

*Warning*
Nest는 @Res / @Next 를 감지하고, 두 방법이 동시에 적용되면 자동으로 비활성화 되어 더이상 작동하지 않음
두 방법을 동시에 적용하려면, 예를들어 쿠키/헤더에 객체를 res를 해줄때 데코레이터에서 passthrough 옵션을 사용해야 한다. 
```ts
@Res({passthrough: true})
@Next()
```

#### @Request 요청 개체
들어오는 (요청오는) 정보를 활용하려면 @Req || @Request 핸들러 서명을 데코레이터에 추가하여 
요청 들어오는 정보에 대한 접근이 가능해진다.

```ts
import {Controller, Get, Req} from '@nestjs/common'
import {Request} from 'express' // npm i @types/express

@Controller('cats')
export class CatsController {
	@Get()
	findAll(@Req() request: Request):string {
		return '../cats 엔드 포인트로 요청들어오는 객체는 @Req 핸들러로 정보에 액세스가 가능해진다.'
	}
}
```

요청개체는 HTTP 요청을 나타내며, 쿼리문자열, 매개변수, HTTP 헤더 및 바디에 대한 속성을 포함 한다.
이러한 요챙개체에 대해서는 수동으로 요청 데이터를 가져올 필요가 없고, 아래는 요청개체에 액세스할 수 있는 
데코레이터 목록이다.
|nestJS|express|설명|
|@Request( ) , @Req( )|request|요청받은 개체에 액세스|
|@Response( ), @Res( )|response| 요청받은 개체에 대한 response 액세스|
|@Next( )|next|요청개체를 다음으로 프로세스로 넘긴다.|
|@Session( )|req.session|세션요청개채에 대한 액세스|
|@Param( key?: string )|req.params, req.params[key]|파라매터 요청개체에 대한 액세스 (key? 는 파라매터 의 키 값으로 들어올지는 모르지만 문자열로 들어온다는 표현)|
|@Body( key?: string )|req.body, req.body[key]|HTTP 바디 요청 개채에 대한 액세스|
|@Query( key?: string )|req.query, req.query[key]| 쿼리요청개체 액세스|
|@Headers( name?: string )|req.headers, req.headers[name]|헤더요청개체 액세스 (name? : string 헤더로 들어오는 name값이 있는지는 모르지만 들어오면 string type으로 들어온다.)|
|@Ip( )|req.ip|요청개채의 ip 값에 대한 액세스|
|@HostParam( )|req.hosts|요청들어오는 호스트 파라매터에 대한 액세스|
[NestJs Official Docs]('https://docs.nestjs.com/controllers')

*기본 HTTP 플랫폼 (예 : Express 및 Fastify)에서 입력과의 호환성을 위해 Nest는 @Res()및 @Response()데코레이터를 제공합니다. `@Res()`는 단순히 `@Response()`의 별칭입니다. 둘 다 기본 HTTP 플랫폼 `response` 객체의 인터페이스를 직접 노출합니다. 하지만 개인적인 경험으로는 `@Response()`보다는 `@Res()`를 사용하시기 바랍니다. `response`객체를 완전하게 활용하기 위해서는 기본 HTTP 플랫폼의 `response`의 타이핑을 가져와야하는데 아래와 같이 `@Response()`와 기본 HTTP 플랫폼의 `response`와 이름이 겹치기 때문입니다*

```ts
import { Controller, Get, Response } from '@nestjs/common'; 
import { Response } from 'express'; // 이 부분에서 에러 

@Controller('cats') export class CatsController { 
@Get() findAll(@Response() res: Response) {} 
} // 아니면 

import { Controller, Get, Response } from '@nestjs/common'; 

@Controller('cats') export class CatsController { 
@Get() findAll(@Response() res: any) {} 
}
```
만약 메서드 핸들러에서 `@Res()`나 `@Respoonse()`를 사용했다면, 위에서 언급한대로 Nest를 `특정 라이브러리 전용` 모드로 사용한다는 것을 뜻하기 때문에 반드시 응답을 관리해야 합니다. 이 경우 `res.json(...)`나 `res.send(...)`로 직접적으로 응답을 주지 않으면 HTTP 서버가 중단되는 참극을 목격하실 수 있습니다.
```ts
import { Controller, Get, Response } from '@nestjs/common'; 
import { Response } from 'express'; // 이 부분에서 에러 

@Controller('cats') export class CatsController { 
@Get() findAll(@Response() res: Response) {
	res.send()
	} 
} // 아니면 

import { Controller, Get, Response } from '@nestjs/common'; 

@Controller('cats') export class CatsController { 
@Get() findAll(@Response() res: any) {
	res.json()
	} 
}
```

[기본 HTTP 플랫폼]('https://www.wisewiredbooks.com/nestjs/overview/02-controller-1.html')

#### Resource
```ts
import {Controller, Get, Post} from '@nestjs/common';

@Controller('cats')
export class CatsController {
	@Post()
	create(): string {
		return '../cats 엔드포인트로 HTTP POST 요청이 들어오는 자원을 생성(create)하겠다.'
	}

	@Get()
	findAll(): string {
		return '../cats 엔드포인트로 HTTP GET 요청이 들어오면 요청 정보를 찾겠다(findAll)'
	}

	@Delete('/:id')
	remove(): number {
		return '../cats/:id 엔드포인트로 HTTP DELETE 요청시 해당 id의 정보를 삭제하겠다' 
	}

	@Put('/:id')
	edit(): number {
		return '../cats/:id 엔드포인트로 HTTP PUT 요청시 해당 id의 정보를 수정하겠다.'
	}

	@Patch('/:id')
	update(): number {
		return '../cats/:id 엔드포인트로 HTTP PUT 요청시 해당 id의 정보를 수정하겠다.'
	}

	@Options('/:id')
	
}
```


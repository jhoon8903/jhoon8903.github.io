---
layout: post
title: Dependency Injection
subtitle: Nest JS 의존성 주입
category: NestJS
author: Daniel
tags: 
 - CS
 - NestJS
 - Programming
banner:
 image : assets/images/posts/slKBeos.jpg
---

DI
--


![](https://i.imgur.com/slKBeos.jpg)

## DI 원칙
- 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
- 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부사항에 의존하지 말아야 한다.

### 의존성 주입과 의존관계역전원칙
- 의존성주입이란 메인모듈이 **'직접'** 다른 **'하위 모듈'** 에 대한 의존성을 주기보다는 중간에 DI가  
  이부분을 가로채 메인 모듈이 **'간접적'** 으로 의존성을 주입하는 방식

### 의존한다?
- A가 B에 의존한다. => B가 변하면 A가 영향을 미치는 관계
- A -> B

---

## NEST JS에서의 의존성 주입

![](https://i.imgur.com/TMc918y.jpg)

---
CODE

```typescript
class Animal{
	private cat: Cat;
	getCrying():string{
		this.cat = new cat()
		return this.cat.Meow()
	}
}

class Cat {
	crying() : string{
		return '야옹'
	}
}
```

```typescript
interface Cry{
	crying() : string;
}

class Animal {
	private animal: Animal;
	getCrying(animal: Animal): string {
		this.animal=animal;
		return this.animal.crying()
	}
}



class Cat implements Cry {
	crying(): string{
		return '야옹'
	}
}

class Dog implements Cry {
	crying(): string {
		return '멍멍'
	}
}
```


```typescript
# cat.interface.ts

export interface Cat {
	name : string ;
	age: string ;
}
```

```typescript
# cat.service.ts

@Injectable
export class CatService {
	private readonly cats: Cat[]=[];
	createCat(cat: Cat) {
		this.cats.push(cat);
	}
	findAll(): Cat[] {
		return this.cats;
	}
}
```

```typescript
# cat.controller.ts

@Controller('cat')
export class catController {
	constructor(private readonly catService: CatService){
	}
	@Post('makeCat')
	makeCat(@Body() cat: Cat){
		return this.catService.createCat(cat)
	}
	@Get('findCat')
	findCat(): Cat[] {
		return this.catservice.findAll()
	}
}
```

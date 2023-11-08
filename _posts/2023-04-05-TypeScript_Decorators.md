---
title: TypeScript 5.0 Decorators
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 Decorators"
---

# ECMAScript 데코레이터를 사용한 코드 재사용성 향상

ECMAScript에서 데코레이터는 클래스와 멤버를 재사용 가능한 방식으로  
사용자 정의 할 수 있는 새로운 기능입니다. 

이 기능을 사용하면 코드를 깔끔하게 유지하고 반복을 줄일 수 있습니다. 

이 글에서는 데코레이터를 사용하여 로깅 및 바인딩과 같은 공통 작업을 처리하는 방법을 살펴보겠습니다.

예를 들어, 클래스의 메서드에 로그를 추가하려면 다음과 같이 작성할 수 있습니다:

```typescript
class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }

    @loggedMethod
    greet() {
        console.log(`Hello, my name is ${this.name}.`);
    }
}
```

`@loggedMethod` 데코레이터를 사용하면 메서드의 시작과 끝에 로그 메시지가 자동으로 추가됩니다.  
데코레이터 함수는 다음과 같이 작성할 수 있습니다:

```typescript
function loggedMethod(originalMethod: any, context: ClassMethodDecoratorContext) {
    const methodName = String(context.name);

    function replacementMethod(this: any, ...args: any[]) {
        console.log(`LOG: Entering method '${methodName}'.`)
        const result = originalMethod.call(this, ...args);
        console.log(`LOG: Exiting method '${methodName}'.`)
        return result;
    }

    return replacementMethod;
}
```

또 다른 유용한 데코레이터는 메서드를 자동으로 바인딩하는 것입니다.  
이를 통해 메서드가 독립된 함수로 호출되거나 콜백으로 전달될 때 `this`가 올바르게 바인딩됩니다.

```typescript
class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }

    @bound
    @loggedMethod
    greet() {
        console.log(`Hello, my name is ${this.name}.`);
    }
}
```

이렇게 데코레이터를 사용하면 코드를 깔끔하게 유지하고 재사용성을 높일 수 있습니다.  
데코레이터는 메서드뿐만 아니라 속성, getter, setter, 자동 접근자 및 클래스 자체에도 사용할 수 있습니다.  
이를 통해 서브클래싱 및 등록과 같은 작업을 처리할 수 있습니다. 

ECMAScript 데코레이터는 코드 재사용성과 유지 관리를 개선하는 데 도움이 되는 강력한 도구입니다.  
이 기능을 활용하면 프로젝트의 구조를 개선하고, 코드 중복을 줄이며, 확장성을 높일 수 있습니다.  
또한 데코레이터를 사용하면 다양한 관심사를 분리하여 관리할 수 있어 코드의 가독성을 높이는 데에도  
도움이 됩니다.


이제 ECMAScript 데코레이터를 사용하여 프로젝트에서 더욱 모듈화된 구조를 만들어 나갈 수 있습니다.  
데코레이터를 사용해 보면서 코드의 유연성과 재사용성을 높이는 법을 찾아보세요.  
이를 통해 팀원들과 협업하며 더 나은 코드 기반을 구축할 수 있을 것입니다.  
데코레이터를 처음 사용하는 개발자들에게는 적응하는 데 시간이 좀 걸릴 수 있지만,  
데코레이터를 익숙하게 사용하게 되면 이 도구가 프로젝트의 생산성을 얼마나 향상시킬 수 있는지   
알게 될 것입니다.
---
title:  TypeScript 5.0 Multiple Conf Files 확장 지원
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 Multiple Configuration Files in extends support"
---


# Multiple Configuration Files in extends Support

여러 프로젝트를 관리할 때, 다른 tsconfig.json 파일이 확장할 수 있는 "기본" 설정 파일이 있으면 
도움이 됩니다. 

그래서 TypeScript는 compilerOptions의 필드를 복사하기 위해 extends 필드를 지원합니다.

```typescript
// packages/front-end/src/tsconfig.json
{
    "extends": "../../../tsconfig.base.json",
    "compilerOptions": {
        "outDir": "../lib",
        // ...
    }
}
```

그러나 여러 설정 파일에서 확장하려는 경우가 있을 수 있습니다.  
예를 들어, [npm에 제공된 TypeScript 기본 설정 파일을 사용한다고 가정](https://github.com/tsconfig/bases)해봅시다.  
모든 프로젝트가 npm의 @tsconfig/strictest 패키지의 옵션도 사용하도록 하려면,  
tsconfig.base.json이 @tsconfig/strictest에서 확장하도록 설정하면 간단한 해결책이 있습니다.

```typescript
// tsconfig.base.json
{
    "extends": "@tsconfig/strictest/tsconfig.json",
    "compilerOptions": {
        // ...
    }
}
```

이 방법은 어느 정도 작동합니다.  
@tsconfig/strictest를 사용하지 않으려는 프로젝트가 있으면, 옵션을 수동으로 비활성화하거나  
@tsconfig/strictest에서 확장하지 않는 tsconfig.base.json의 별도 버전을 만들어야 합니다.

여기서 더 많은 유연성을 제공하기 위해, TypeScript 5.0은 이제 extends 필드가 여러 항목을   
사용할 수 있습니다. 

예를 들어, 이 설정 파일에서:
```typescript
{
    "extends": ["a", "b", "c"],
    "compilerOptions": {
        // ...
    }
}
```
이것을 작성하는 것은 마치 c를 직접 확장하는 것처럼 보이는데, 여기서 c는 b를 확장하고, b는 a를 확장합니다.    
어떤 필드가 "충돌"하면, 나중에 있는 항목이 이깁니다.

따라서 다음 예에서 최종 tsconfig.json에서 strictNullChecks와 noImplicitAny가 모두 활성화됩니다.

```typescript
// tsconfig1.json
{
    "compilerOptions": {
        "strictNullChecks": true
    }
}

// tsconfig2.json
{
    "compilerOptions": {
        "noImplicitAny": true
    }
}

// tsconfig.json
{
    "extends": ["./tsconfig1.json", "./tsconfig2.json"],
    "files": ["./index.ts"]
}
```

다른 예로, 원래의 예제를 다음과 같이 다시 작성할 수 있습니다.

```typescript
// packages/front-end/src/tsconfig.json
{
    "extends": ["@tsconfig/strictest/tsconfig.json", "../../../tsconfig.base.json"],
    "compilerOptions": {
        "outDir": "../lib",
        // ...
    }
}
```

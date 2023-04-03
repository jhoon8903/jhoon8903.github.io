---
title: TypeScript 5.0 중요 변경 사항 및 사용 중단
title: TypeScript 5.0 속도, 메모리, 패키지 크기 최적화
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 중요 변경 사항 및 사용 중단"
---
# TypeScript 5.0 중요 변경 사항 및 사용 중단

런타임 요구 사항 TypeScript는 이제 ECMAScript 2018을 대상으로 합니다. 
또한 TypeScript 패키지는 최소 12.20 엔진을 기대하도록 설정되었습니다. 
Node 사용자의 경우 TypeScript 5.0은 최소한 Node.js 12.20 이상의 버전을 필요로 합니다.

lib.d.ts 변경 사항 DOM에 대한 유형을 생성하는 방법의 변경으로 인해 기존 코드에 영향을 줄 수 있습니다. 
특히, 일부 속성이 숫자에서 숫자 리터럴 유형으로 변환되었고, 잘라내기, 복사, 붙여넣기 이벤트 처리를 위한 
속성과 메서드가 인터페이스 간에 이동되었습니다.

API 중단 변경 사항 TypeScript 5.0에서는 모듈로 이동하고 불필요한 인터페이스를 제거하며 일부 정확성 개선을 수행했습니다. 

변경 사항에 대한 자세한 내용은 [API Breaking Changes 페이지](https://github.com/microsoft/TypeScript/wiki/API-Breaking-Changes)를 참조하세요.

관계 연산자에서 금지된 암시적 형변환 TypeScript의 특정 작업에서 문자열에서 숫자로의 암시적 형변환이 발생할 수 있는 코드를 작성하면 경고가 표시됩니다.

```typescript
function func(ns: number | string) {
  return ns * 4; // Error, possible implicit coercion
}
```

5.0에서 이것은 >, <, <= 및 >=와 같은 관계 연산자에도 적용됩니다.

```typescript
function func(ns: number | string) {
  return ns > 4; // Now also an error
}
```

원하는 경우, 숫자로 명시적으로 형변환하여 허용할 수 있습니다.

```typescript
function func(ns: number | string) {
  return +ns > 4; // OK
}
```

Enum 개선 
TypeScript는 처음 출시 이후부터 enum에 관한 몇 가지 오류가 있었습니다. 
5.0에서는 이러한 문제를 해결하고 선언할 수 있는 다양한 유형의 enum을 이해하는 데 필요한 
개념 수를 줄입니다.

이와 관련하여 두 가지 주요 새로운 오류가 발생할 수 있습니다. 

첫째, 열거형 유형에 도메인 외 리터럴을 할당하면 예상대로 오류가 발생합니다.

```typescript
enum SomeEvenDigit {
    Zero = 0,
    Two = 2,
    Four = 4
}

// Now correctly an error
let m: SomeEvenDigit = 1;
```

또 다른 문제는 숫자와 간접 문자열 열거형 참조로 선언된 값의 혼합을 사용한 열거형은 
잘못된 전체 숫자 열거형을 생성하였습니다.

```typescript
enum Letters {
    A = "a"
}
enum Numbers {
    one = 1,
    two = Letters.A
}

// Now correctly an error
const t: number = Numbers.two;
```

자세한 내용은 [관련 변경 사항에서 확인](https://github.com/microsoft/TypeScript/pull/50528)할 수 있습니다.

## TypeScript 5.0에서 더 정확한 타입 검사 및 업데이트된 기본값과 함께 몇 가지 변경 사항이 있습니다.

**더 정확한 Parameter Decorators 타입 검사** 
TypeScript 5.0은 --experimentalDecorators 아래에서 데코레이터에 대한 타입 검사를 더 정확하게 
만듭니다. 

생성자 매개변수에서 데코레이터를 사용할 때 이 점이 두드러집니다.

```typescript
export declare const inject:
  (entity: any) =>
    (target: object, key: string | symbol, index?: number) => void;

export class Foo {}

export class C {
    constructor(@inject(Foo) private x: any) {
    }
}
```

이 호출은 생성자 매개변수가 undefined 키를 받지만, key는 string | symbol이기 때문에 실패합니다. 
올바른 수정은 inject 내의 key 타입을 변경하는 것입니다. 
업그레이드 할 수 없는 라이브러리를 사용하는 경우에는 key에 대한 타입 단언을 사용하여 inject를 더 
타입-안전한 데코레이터 함수로 감싸는 것이 합리적인 해결책입니다. 

자세한 내용은 [이 이슈를 참조](https://github.com/microsoft/TypeScript/issues/52435)하세요.

**사용 중단 및 기본값 변경** TypeScript 5.0에서는 다음 설정과 설정 값이 사용 중단되었습니다.

5.0에서는 몇 가지 설정들이 제거될 예정입니다. 이러한 설정들은 아래와 같습니다.

--target: ES3 
--out 
--noImplicitUseStrict 
--keyofStringsOnly 
--suppressExcessPropertyErrors 
--suppressImplicitAnyIndexErrors 
--noStrictGenericChecks 
--charset 
--importsNotUsedAsValues 
--preserveValueImports 
prepend in project references 

이러한 설정들은 TypeScript 5.5에서 완전히 제거될 예정이지만, 경고 메시지가 표시됩니다. 
TypeScript 5.0부터 5.4까지 "ignoreDeprecations": "5.0"를 지정하여 이러한 경고 메시지를 
숨길 수 있습니다. 

또한 4.9 패치를 곧 출시하여 업그레이드를 보다 원활하게 할 수 있도록 할 예정입니다.

이외에도, TypeScript에서는 여러 가지 설정을 변경하여 크로스 플랫폼 환경에서 더욱 잘 작동하도록 
개선했습니다. 

예를 들면,

--newLine: 이 설정은 JavaScript 파일에서 사용되는 줄 바꿈 문자를 제어합니다. 
이전에는 이 값을 지정하지 않으면 현재 운영체제에 따라 유추됩니다. 
이제는 Windows Notepad에서도 LF 줄 바꿈 문자를 지원하므로, 더욱 결정적인 빌드를 위해 이 설정의 
기본값이 LF로 변경되었습니다.

--forceConsistentCasingInFileNames: 이 설정은 프로젝트 내에서 같은 파일 이름을 참조하는 경우 
대소문자를 일관되게 사용하는지 확인합니다. 
이제 기본값이 true로 변경되어, 대소문자를 구분하지 않는 파일 시스템에서 작성된 코드와의 차이를 
발견할 수 있습니다.

TypeScript 5.0의 변경 사항 및 제거될 설정에 대한 자세한 내용은 [여기에서 확인](https://github.com/microsoft/TypeScript/issues/51909)할 수 있습니다.
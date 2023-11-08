---
layout: post
title: TypeScript 5.0 const Type Parameters
subtitle: TypeScript 5.0 Update Notice
categories: TypeScript
author: Daniel
tags: 
 - TypeScript
banner:
 image : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
---

## TypeScript에서 const Type Parameters 사용하기

TypeScript에서 객체의 유형을 추론할 때 일반적으로 공통적인 유형을 선택합니다. 
예를 들어, 다음 예제에서 `names`의 추론된 유형은 `string[]`입니다.

```typescript
type HasNames = { readonly names: string[] };
function getNamesExactly<T extends HasNames>(arg: T): T["names"] {
    return arg.names;
}

// 추론된 유형: string[]
const names = getNamesExactly({ names: ["Alice", "Bob", "Eve"]});
```

이것은 보통 후에 발생하는 변형을 가능하게 하기 위한 의도입니다.

그러나 `getNamesExactly`의 사용법에 따라, 더 구체적인 유형이 원하는 경우가 종종 있습니다.

지금까지 API 작성자들은 원하는 추론을 얻기 위해 특정 위치에 `as const`를 추가하는 것을 권장해 왔습니다.

```typescript
// 원하는 유형:
//    readonly ["Alice", "Bob", "Eve"]
// 받은 유형:
//    string[]
const names1 = getNamesExactly({ names: ["Alice", "Bob", "Eve"]});

// 원하는 것을 정확하게 얻음:
//    readonly ["Alice", "Bob", "Eve"]
const names2 = getNamesExactly({ names: ["Alice", "Bob", "Eve"]} as const);
```

이 방법은 번거롭고 잊어버리기 쉽습니다.  
TypeScript 5.0에서는 이제 타입 매개 변수 선언에 `const` 수정자를 추가하여 기본적으로 const와   
유사한 추론이 이루어지도록 할 수 있습니다.

```typescript
type HasNames = { names: readonly string[] };
function getNamesExactly<const T extends HasNames>(arg: T): T["names"] {
//                       ^^^^^
    return arg.names;
}

// 추론된 유형: readonly ["Alice", "Bob", "Eve"]
// 참고: 여기서 'as const'를 작성할 필요 없음
const names = getNamesExactly({ names: ["Alice", "Bob", "Eve"] });
```

`const` 수정자는 변경 가능한 값을 거부하지 않으며, 불변 제약 조건이 필요하지 않습니다.   
변경 가능한 타입 제약을 사용하면 예상치 못한 결과가 나올 수 있습니다.

```typescript
declare function fnBad<const T extends string[]>(args: T): void;

// 'T'는 여전히 'string[]'입니다. 'readonly ["a", "b", "c"]'는 'string[]'에 할당할 수 없습니다.
fnBad(["a", "b" ,"c"]);
```

이 경우, 추론은 제약 조건으로 되돌아가 배열이 `string[]`로 취급되고, 호출은 여전히 성공적으로 진행됩니다.

이 함수의 더 나은 정의는 `readonly string[]`를 사용해야 합니다.

```typescript
declare function fnGood<const T extends readonly string[]>(args: T): void;

// T는 readonly ["a", "b", "c"]
fnGood(["a", "b" ,"c"]);
```

비슷하게, `const` 수정자가 호출 내에 작성된 객체, 배열 및 기본 표현식의 추론에만 영향을 미치므로,  
 `as const` 로 수정되지 않거나 수정할 수 없는 인수에는 동작이 변경되지 않습니다.

```typescript
declare function fnGood<const T extends readonly string[]>(args: T): void;
const arr = ["a", "b" ,"c"];

// 'T'는 여전히 'string[]'입니다. 'const' 수정자는 여기에서 효과가 없습니다.
fnGood(arr);
```

자세한 내용은 [풀 리퀘스트](https://github.com/microsoft/TypeScript/pull/51865) 및 ([첫 번째](https://github.com/microsoft/TypeScript/issues/30680) 및 [두 번째](https://github.com/microsoft/TypeScript/issues/41114)) 동기 부여 이슈를 참조하세요.
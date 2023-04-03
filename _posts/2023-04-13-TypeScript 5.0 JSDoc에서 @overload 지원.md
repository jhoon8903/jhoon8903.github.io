---
title: TypeScript 5.0 JSDoc에서 @overload 지원
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 JSDoc에서 @overload 지원"
---
# TypeScript 5.0 JSDoc에서 @overload 지원

TypeScript에서는 함수에 대한 @overload를 지정할 수 있습니다. 
@overload는 함수가 다른 인수로 호출될 수 있고, 가능하면 다른 결과를 반환한다는 것을 나타냅니다. 
@overload를 사용하면 함수를 어떻게 사용할 수 있는지 제한하고 반환되는 결과를 세밀하게 조정할 수 
있습니다.

```typescript
// 오버로드:
function printValue(str: string): void;
function printValue(num: number, maxFractionDigits?: number): void;

// 구현:
function printValue(value: string | number, maximumFractionDigits?: number) {
    if (typeof value === "number") {
        const formatter = Intl.NumberFormat("en-US", {
            maximumFractionDigits,
        });
        value = formatter.format(value);
    }

    console.log(value);
}
```

여기서 printValue는 첫 번째 인수로 문자열 또는 숫자를 사용한다고 설명했습니다. 
숫자를 사용하는 경우 두 번째 인수로 출력할 소수점 자릿수를 결정할 수 있습니다.

TypeScript 5.0은 이제 JSDoc에서 새로운 @overload 태그를 사용하여 오버로드를 선언할 수 있게 합니다. @overload 태그가 있는 각 JSDoc 주석은 이후의 함수 선언에 대한 별도의 오버로드로 처리됩니다.

```typescript
// @ts-check

/**
 * @overload
 * @param {string} value
 * @return {void}
 */

/**
 * @overload
 * @param {number} value
 * @param {number} [maximumFractionDigits]
 * @return {void}
 */

/**
 * @param {string | number} value
 * @param {number} [maximumFractionDigits]
 */
function printValue(value, maximumFractionDigits) {
    if (typeof value === "number") {
        const formatter = Intl.NumberFormat("en-US", {
            maximumFractionDigits,
        });
        value = formatter.format(value);
    }

    console.log(value);
}
```

이제 TypeScript 파일이나 JavaScript 파일에서 작성하든 상관없이, TypeScript는 함수가 잘못 호출되었는지 여부를 알려줄 수 있습니다.

```typescript
// 모두 허용됨
printValue("hello!");
printValue(123.45);
printValue(123.45, 2);

printValue("hello!", 123); // 에러!
```
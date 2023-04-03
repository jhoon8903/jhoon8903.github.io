---
title: TypeScript 5.0 JSDoc에서 @satisfies 지원
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 JSDoc에서 @satisfies 지원"
---

# JSDoc에서 @satisfies 지원

TypeScript 4.9에서는 satisfies 연산자가 도입되어 표현식의 타입이 호환되는지 확인했습니다. 

예를 들어 아래 코드를 살펴봅시다:

```typescript
interface CompilerOptions {
    strict?: boolean;
    outDir?: string;
    // ...
}

interface ConfigSettings {
    compilerOptions?: CompilerOptions;
    extends?: string | string[];
    // ...
}

let myConfigSettings = {
    compilerOptions: {
        strict: true,
        outDir: "../lib",
        // ...
    },

    extends: [
        "@tsconfig/strictest/tsconfig.json",
        "../../../tsconfig.base.json"
    ],

} satisfies ConfigSettings;
```

TypeScript는 myConfigSettings.extends가 배열로 선언되었음을 알고 있습니다. 
그 이유는 satisfies가 객체의 타입을 검증하면서도 ConfigSettings로 변경하지 않아 정보 손실이 
발생하지 않기 때문입니다. 따라서 extends를 매핑할 수 있습니다.

```typescript
declare function resolveConfig(configPath: string): CompilerOptions;

let inheritedConfigs = myConfigSettings.extends.map(resolveConfig);
```

TypeScript 사용자에게 유용했지만, 많은 사람들이 JSDoc 주석을 사용하여 
JavaScript 코드를 타입 검사하기 위해 TypeScript를 사용합니다. 
그래서 TypeScript 5.0은 동일한 작업을 수행하는 새로운 JSDoc 태그인 @satisfies를 지원합니다.

`@satisfies`는 타입 불일치를 찾아냅니다:
```typescript
// @ts-check

/**
 * @typedef CompilerOptions
 * @prop {boolean} [strict]
 * @prop {string} [outDir]
 */

/**
 * @satisfies {CompilerOptions}
 */
let myCompilerOptions = {
    outdir: "../lib",
//  ~~~~~~ oops! we meant outDir
};
```

하지만 원래 표현식의 타입을 유지하여 코드에서 값들을 더 정확하게 사용할 수 있습니다.

```typescritp
// @ts-check

/**
 * @typedef CompilerOptions
 * @prop {boolean} [strict]
 * @prop {string} [outDir]
 */

/**
 * @typedef ConfigSettings
 * @prop {CompilerOptions} [compilerOptions]
 * @prop {string | string[]} [extends]
 */


/**
 * @satisfies {ConfigSettings}
 */
```
```typescript
let myConfigSettings = {
    compilerOptions: {
        strict: true,
        outDir: "../lib",
    },
    extends: [
        "@tsconfig/strictest/tsconfig.json",
        "../../../tsconfig.base.json"
    ],
};

let inheritedConfigs = myConfigSettings.extends.map(resolveConfig);
```

`@satisfies`는 소괄호로 묶인 표현식에서도 인라인으로 사용할 수 있습니다. 

이렇게 myConfigSettings를 작성할 수 있습니다:
```typescript
let myConfigSettings = /** @satisfies {ConfigSettings} */ ({
    compilerOptions: {
        strict: true,
        outDir: "../lib",
    },
    extends: [
        "@tsconfig/strictest/tsconfig.json",
        "../../../tsconfig.base.json"
    ],
});
```

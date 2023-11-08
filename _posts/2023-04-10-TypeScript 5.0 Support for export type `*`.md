---
layout: post
title: TypeScript 5.0 Support for export type `*`
subtitle: TypeScript 5.0 Update Notice
categories: TypeScript
author: Daniel
tags: 
 - TypeScript
banner:
 image : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
---

## TypeScript 5.0에서 type-only 내보내기 및 다시 내보내기 지원

TypeScript 3.8에서 type-only 가져오기가 도입되었을 때, 새로운 구문은 export * from "module" 또는 export * as ns from "module"의 다시 내보내기에는 허용되지 않았습니다. TypeScript 5.0은 이러한 두 가지 형식을 모두 지원합니다:

```typescript
// models/vehicles.ts
export class Spaceship {
  // ...
}

// models/index.ts
export type * as vehicles from "./vehicles";

// main.ts
import { vehicles } from "./models";

function takeASpaceship(s: vehicles.Spaceship) {
  //  ok - `vehicles` only used in a type position
}

function makeASpaceship() {
  return new vehicles.Spaceship();
  //         ^^^^^^^^
  // 'vehicles' cannot be used as a value because it was exported using 'export type'.
}
```

이 기능을 사용하면, 타입 위치에서만 사용되는 type-only 내보내기 및 다시 내보내기를 작성할 수 있습니다. 
이렇게 하면 실행 시간 오류를 줄일 수 있고, 타입과 값의 사용을 명확하게 구분할 수 있습니다.

자세한 내용은 [구현에 대한 설명을 참조](https://github.com/microsoft/TypeScript/pull/52217)하세요.
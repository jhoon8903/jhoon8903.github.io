---
title: TypeScript 5.0 대소문자 구분 없는 Import 정렬 기능 추가
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 대소문자 구분 없는 Import 정렬 기능 추가"
---

Visual Studio와 VS Code와 같은 에디터에서는 TypeScript가 import와 export를 정리하고 정렬하는 경험을 제공합니다. 

그러나 종종 목록이 "정렬됨"인지에 대한 해석이 다를 수 있습니다.

예를 들어, 아래 import 목록이 정렬되어 있다고 할 수 있을까요?

```typescript
import {
    Toggle,
    freeze,
    toBoolean,
} from "./utils";
```

놀랍게도 답은 "상황에 따라 다르다"입니다. 
대소문자 구분을 하지 않는다면, 이 목록은 분명 정렬되지 않았습니다. 
f는 t와 T보다 앞섭니다.

그러나 대부분의 프로그래밍 언어에서 정렬은 문자열의 바이트 값을 비교하는 것으로 기본 설정됩니다. JavaScript에서 문자열을 비교하는 방식은 ASCII 문자 인코딩에 따라 대문자가 소문자보다 앞에 옵니다. 
그 관점에서 보면 import 목록은 정렬되어 있습니다.

이전 TypeScript는 기본적으로 대소문자 구분 정렬을 사용하여 import 목록이 정렬되어 있다고 간주했습니다. 이는 대소문자 구분 없는 순서를 선호하는 개발자들이나 기본적으로 대소문자 구분 없는 정렬을 요구하는 
ESLint와 같은 도구를 사용하는 경우 불만의 원인이 될 수 있습니다.

이제 TypeScript는 기본적으로 대소문자 구분 여부를 감지합니다. 
이는 TypeScript와 ESLint와 같은 도구가 import를 어떻게 정렬할지 서로 "싸우지" 않게 됩니다.

현재는 아직 불안정하고 실험적이며, VS Code에서 JSON 옵션에서 `typescript.unstable` 항목을 
사용하여 활성화할 수 있습니다. 

아래에 모든 옵션과 기본값이 나열되어 있습니다:
```typescript
{
    "typescript.unstable": {
        "organizeImportsIgnoreCase": "auto",
        "organizeImportsCollation": "ordinal",
        "organizeImportsLocale": "en",
        "organizeImportsCaseFirst": false,
        "organizeImportsNumericCollation": true,
        "organizeImportsAccentCollation": true
    },
    "javascript.unstable": {
        // 같은 옵션 유효...
    },
}
```

대소문자 구분 없음을 자동 감지하고 지정하는 작업에 대한 자세한 정보와 더 넓은 범위의 옵션에 대해서는 
[여기에서 확인](https://github.com/microsoft/TypeScript/pull/52115) 가능합니다.


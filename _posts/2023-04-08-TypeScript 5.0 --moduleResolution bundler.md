---
title: TypeScript 5.0 --moduleResolution bundler
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 --moduleResolution bundler"
---

# --moduleResolution bundler

TypeScript 4.7에서는 Node.js의 ECMAScript 모듈에 대한 정확한 탐색 규칙을 더 잘 모델링하기 위해 
--module 및 --moduleResolution 설정에 대해 node16 및 nodenext 옵션을 도입했습니다. 

그러나 이 모드에는 다른 도구에서 실제로 적용하지 않는 많은 제한이 있습니다.

예를 들어, Node.js의 ECMAScript 모듈에서 상대적인 가져오기는 파일 확장자를 포함해야 합니다.

```typescript
// entry.mjs 
import * as utils from "./utils";     //  wrong - we need to include the file extension.  
import * as utils from "./utils.mjs"; //  works`
```


Node.js와 브라우저에서 파일 조회를 더 빠르게 하고 간단한 파일 서버에 더 적합하게 만드는 이유가 있습니다. 
그러나 번들러와 같은 도구를 사용하는 많은 개발자들에게는 node16/nodenext 설정이 번거로웠습니다. 
번들러는 대부분의 제한이 없기 때문입니다. 
어떤 면에서는 node 해상도 모드가 번들러를 사용하는 데 더 적합했습니다.

그러나 어떤 면에서는 원래의 node 해상도 모드가 이미 구식이었습니다. 
현대의 번들러는 Node.js의 ECMAScript 모듈과 CommonJS 조회 규칙을 결합합니다. 
예를 들어, 확장자가 없는 가져오기는 CommonJS처럼 잘 작동하지만 패키지의 내보내기 조건을 검토할 때는 ECMAScript 파일처럼 import 조건을 선호합니다.

번들러가 작동하는 방식을 모델링하기 위해 TypeScript는 이제 새로운 전략을 도입합니다: 

```typescript
--moduleResolution bundler.

{
    "compilerOptions": {
        "target": "esnext",
        "moduleResolution": "bundler"
    }
}
```


Vite, esbuild, swc, Webpack, Parcel 등 하이브리드 조회 전략을 구현하는 현대적인 번들러를 사용하는 경우, 새로운 bundler 옵션을 사용하면 좋습니다.

반면에, npm에 게시할 라이브러리를 작성하는 경우, bundler 옵션을 사용하면 번들러를 사용하지 않는 사용자에게 발생할 수 있는 호환성 문제가 숨겨질 수 있습니다. 따라서 이러한 경우에는 node16 또는 nodenext 해상도 옵션을 사용하는 것이 더 나은 방법일 것입니다.

--moduleResolution bundler에 대해 자세히 알아보려면, 구현하는 [pull request를 참조](https://github.com/microsoft/TypeScript/pull/51669)하세요.
---
title: TypeScript_5.0_추가된 내용
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 추가된 내용"
---


# beta 및 RC 이후 추가된 내용

### 1. 데코레이터 위치 변경 허용

TypeScript 5.0에서는 데코레이터가 `export`와 `export default` 앞뒤에 위치할 수 있도록 허용합니다. 이 변경 사항은 ECMAScript/JavaScript 표준을 관리하는 TC39에서의 논의와 합의를 반영한 것입니다.

### 2. 새로운 번들러 모듈 해석 옵션 변경

새로운 번들러 모듈 해석 옵션은 이제 `--module` 옵션이 `esnext`로 설정된 경우에만 사용할 수 있습니다. 이는 입력 파일에서 작성된 `import` 문이 번들러가 해석하기 전에 `require` 호출로 변환되지 않도록 하기 위한 것이며, 번들러나 로더가 TypeScript의 모듈 옵션을 따르는지 여부와 관계없습니다. 또한, 대부분의 라이브러리 작성자들이 `node16` 또는 `nodenext`를 사용하도록 권장하는 내용을 릴리즈 노트에 추가했습니다.

### 3. 대소문자 구분 없는 `import` 정렬 지원

TypeScript 5.0 베타에는 대소문자 구분 없는 `import` 정렬을 에디터에서 지원하는 기능이 포함되어 있었지만 문서화되지 않았습니다. 이는 사용자 경험에 대한 논의가 진행 중이기 때문이며, 기본적으로 TypeScript는 이제 다른 도구와 더 잘 호환됩니다.

### 4. 최소 Node.js 버전 12.20 지정

RC 이후 가장 주목할 만한 변경 사항 중 하나는 TypeScript 5.0에서 `package.json`에 최소 Node.js 버전을 12.20으로 지정한 것입니다. 또한, TypeScript 5.0의 모듈로의 이전에 대한 글을 작성하고 링크를 제공했습니다.

### 5. 속도 벤치마크 및 패키지 크기 조정

TypeScript 5.0 베타와 RC가 발표된 이후, 속도 벤치마크와 패키지 크기 차이에 대한 구체적인 수치가 조정되었습니다. 실행간의 노이즈가 요인으로 작용했습니다. 또한, 몇몇 벤치마크의 이름이 명확성을 위해 조정되었고, 패키지 크기 개선 사항이 별도의 차트로 이동되었습니다.


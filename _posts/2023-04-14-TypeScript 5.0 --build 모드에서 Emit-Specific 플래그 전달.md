---
title: TypeScript 5.0 --build 모드에서 Emit-Specific 플래그 전달
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 --build 모드에서 Emit-Specific 플래그 전달"
---

# `--build` 모드에서 Emit-Specific 플래그 전달

TypeScript는 이제 다음 플래그를 `--build` 모드에서 전달할 수 있습니다.

-   `--declaration`
-   `--emitDeclarationOnly`
-   `--declarationMap`
-   `--sourceMap`
-   `--inlineSourceMap`

이를 통해 개발 및 프로덕션 빌드에서 다른 부분을 쉽게 사용자 정의할 수 있습니다.

예를 들어, 라이브러리의 개발 빌드는 선언 파일을 생성할 필요가 없지만, 프로덕션 빌드는 필요합니다. 
프로젝트는 기본적으로 선언 생성을 꺼두고 간단하게 다음과 같이 빌드할 수 있습니다.

```typescript
tsc --build -p ./my-project-dir
```

내부 루프에서 반복 작업을 완료하면 "프로덕션" 빌드는 `--declaration` 플래그만 전달하면 됩니다.
```typescript
tsc --build -p ./my-project-dir --declaration
```

이 변경에 대한 자세한 정보는 [여기에서 확인](https://github.com/microsoft/TypeScript/pull/51241)할 수 있습니다.
---
title: TypeScript 5.0 완전한 switch_case 자동완성 기능 추가
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 완전한 switch_case 자동완성 기능 추가"
---

# TypeScript 5.0 완전한 switch/case 자동완성 기능 추가

switch 문을 작성할 때 TypeScript는 이제 검사되는 값이 리터럴 타입인지 확인합니다. 
만약 그렇다면, 각각의 덮여있지 않은 케이스를 구성할 수 있는 완성을 제안합니다. 
이를 통해 사용자는 빠르게 모든 경우를 처리하는 switch/case 문을 작성할 수 있습니다. 

이 기능은 코드 작성 시간을 줄이고 누락된 경우를 처리하는 데 도움이 됩니다.

![](https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/exhaustiveCaseCompletions-5.0-stable-1.gif)

자세한 내용은 [이곳에서 확인](https://github.com/microsoft/TypeScript/pull/50996) 가능합니다.
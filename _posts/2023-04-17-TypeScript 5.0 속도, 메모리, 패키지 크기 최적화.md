---
title: TypeScript 5.0 속도, 메모리, 패키지 크기 최적화
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 속도, 메모리, 패키지 크기 최적화"
---

# 속도, 메모리, 패키지 크기 최적화

TypeScript 5.0에는 코드 구조, 데이터 구조 및 알고리즘 구현에 걸쳐 다양한 강력한 변화가 포함되어 있습니다. 이 모든 것들이 사용자의 전체 경험을 더 빠르게 만들어주는데, TypeScript를 실행하는 것뿐만 아니라 설치하는 것까지 더 빠른 경험을 제공합니다.

다음은 TypeScript 4.9와 비교하여 속도와 크기에서 얻을 수 있는 몇 가지 흥미로운 결과입니다.

|시나리오|TypeScript 4.9 대비 시간 또는 크기|
|:----:|:----:|
|material-ui 빌드 시간|90%|
|TypeScript 컴파일러 시작 시간|89%|
|Playwright 빌드 시간|88%|
|TypeScript 컴파일러 자체 빌드 시간|87%|
|Outlook 웹 빌드 시간|82%|
|VS Code 빌드 시간|80%|
|typescript npm 패키지 크기|59%|
![](https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/speed-5.0-stable-2.png)
![](https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/size-5.0-stable-1.png)
빠른 속도와 작은 크기를 얻기 위해 어떻게 최적화를 수행했는지 앞으로 자세한 설명을 제공할 예정입니다. 

그러나 이 블로그 포스트를 기다리지 않고도 소개해드리겠습니다.

먼저, TypeScript를 네임스페이스에서 모듈로 마이그레이션하여, 스코프 호이스팅과 같은 최적화를 수행할 수 있는 현대 빌드 도구를 활용할 수 있게 되었습니다. 

이 도구를 사용하고 패키징 전략을 재검토하고 더 이상 사용되지 않는 코드를 제거함으로써 TypeScript 4.9의 63.8 MB 패키지 크기에서 약 26.4 MB를 줄일 수 있었습니다. 

또한 직접 함수 호출을 통해 속도를 향상시킬 수 있었습니다. 

모듈로의 마이그레이션에 대한 자세한 설명은 여기에서 확인할 수 있습니다.

또한 TypeScript는 컴파일러 내부의 객체 타입에 대한 일관성을 높이고, 일부 객체 타입에서 저장되는 데이터를 줄였습니다. 

이렇게 하여 다형적 작업을 줄이면서 객체 모양을 보다 균일하게 만드는 데 따른 메모리 사용 증가를 균형잡았습니다.

또한 문자열로 정보를 직렬화할 때 일부 캐싱을 수행했습니다. 

오류 보고, 선언 방출, 코드 완성 등에서 발생할 수 있는 타입 표시와 같은 작업이 상당한 비용이 들 수 있습니다. TypeScript는 이러한 작업 전반에 걸쳐 일반적으로 사용되는 일부 구조를 캐시하여 재사용할 수 있도록 했습니다.

또 다른 주목할 만한 변경 사항은 클로저에서 let과 const의 사용 비용을 피하기 위해 가끔 var를 사용하는 것으로 구문 분석 성능을 개선했습니다.

전반적으로 TypeScript 5.0에서 대부분의 코드베이스는 속도 향상을 경험할 것으로 예상되며, 10%에서 20% 사이의 향상을 지속적으로 재현할 수 있습니다. 

물론 하드웨어 및 코드베이스 특성에 따라 다르겠지만, 오늘 여러분의 코드베이스에서 시도해 보시기 바랍니다!

더 자세한 정보는 다음의 주요 최적화 사항을 확인하세요:

-  [Migrate to Modules](https://github.com/microsoft/TypeScript/pull/51387)
-  [`Node` Monomorphization](https://github.com/microsoft/TypeScript/pull/51682)
-  [`Symbol` Monomorphization](https://github.com/microsoft/TypeScript/pull/51880)
-  [`Identifier` Size Reduction](https://github.com/microsoft/TypeScript/pull/52170)
-  [`Printer` Caching](https://github.com/microsoft/TypeScript/pull/52382)
-  [Limited Usage of `var`](https://github.com/microsoft/TypeScript/issues/52924)
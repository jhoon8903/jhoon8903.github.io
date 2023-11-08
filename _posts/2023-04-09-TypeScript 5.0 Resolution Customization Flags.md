---
layout: post
title: TypeScript 5.0 Resolution Customization Flags
subtitle: TypeScript 5.0 Update Notice
categories: TypeScript
author: Daniel
tags: 
 - TypeScript
banner:
 image : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
---

## TypeScript 5.0의  Resolution Customization Flags 기능

JavaScript 도구는 이제 위에서 설명한 번들러 모드와 같은 "하이브리드" resolution rules을 모델링할 수 있습니다. 

도구들이 약간씩 다른 지원을 제공할 수 있기 때문에, TypeScript 5.0은 구성과 함께 작동 여부에 따라 몇 가지 기능을 활성화하거나 비활성화할 수 있는 방법을 제공합니다.

1.  allowImportingTsExtensions --allowImportingTsExtensions는 .ts, .mts 또는 .tsx와 같은 TypeScript 전용 확장자로 서로를 가져올 수 있게 합니다.
    
2.  resolvePackageJsonExports --resolvePackageJsonExports는 TypeScript가 node_modules 내의 패키지에서 읽을 때 package.json 파일의 exports 필드를 참조하도록 강제합니다.
    
3.  resolvePackageJsonImports --resolvePackageJsonImports는 패키지.json의 imports 필드를 참조하도록 강제합니다.
    
4.  allowArbitraryExtensions TypeScript 5.0에서는, CSS 로더와 같은 번들러 프로젝트에서 스타일시트의 선언 파일을 작성하거나 생성할 수 있습니다.
    
5.  customConditions --customConditions는 TypeScript가 package.json의 exports 또는 imports 필드를 해석할 때 사용할 추가 조건을 설정할 수 있습니다.
    
6.  --verbatimModuleSyntax TypeScript 5.0에서는 --verbatimModuleSyntax 옵션을 도입하여 상황을 단순화합니다. 이 옵션을 사용하면, type 수정자가 없는 모든 가져오기와 내보내기가 그대로 남아있고, type 수정자를 사용하는 것은 완전히 삭제됩니다.
    

이 새로운 옵션을 사용하면 코드에서 볼 수 있는 것이 그대로 표시됩니다. 

그러나 이렇게 하면 모듈 간 호환성에 대한 일부 영향이 있습니다. 

이 플래그를 사용하면, ECMAScript 가져오기 및 내보내기가 require 호출로 다시 작성되지 않습니다.

이 변경으로 인해 기존의 --importsNotUsedAsValues와 --preserveValueImports 플래그는 
더 이상 사용되지 않게 되며, 이를 대신하여 --verbatimModuleSyntax 옵션이 제공됩니다.

자세한 내용은 원래의 [pull request](https://github.com/microsoft/TypeScript/pull/52203)와 [이슈](https://github.com/microsoft/TypeScript/issues/51479)를 참조하세요.
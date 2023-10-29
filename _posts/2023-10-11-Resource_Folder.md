---
title: Unity Resources Folder
category: Unity
author: 이정훈
tags: [Unity, Programming]
img: https://i.imgur.com/ULdv62G.jpg
comments_disable: true
meta_description: Unity Resources Folder
---
# Resources Folder
- Run Time 에서 Script를 통해 직접 로드가 가능해짐
- Folder Name은 "Resources"로 철자가 틀리면 안된다.
⚠️ Resources Folder 내에 있는 모든 에셋은 런타임에 직접 불러와 사용할 수 있지만, 
    빌드할 떄 실행 파일에 모두 포함됨, 게임에 사용하지 않는 리소스가 있으면 안됨

![](https://i.imgur.com/ULdv62G.jpg)
```csharp
// Blood Object  
private GameObject _bloodEffect;

// Resource Folder에서 Prefabs Load
_bloodEffect = Resources.Load<GameObject>("BloodSprayEffect");
```

- Resources 폴더에서 에셋의 타입에 따라 Sub Folder 구조라면, Asset Name 까지 지정해야함
```csharp
|        폴더         | 로드할 에셋 | 문법  
| Resources/Prefabs/ | Monster  | Resources.Load<GameObject>("Prefabs/Monster");
| Resources/Sounds/  | gunFire  | Resources.Load<AudioClip>("Sounds/gunFire");
```




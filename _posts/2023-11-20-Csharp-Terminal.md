---
layout: post
title: C# Mac Terminal Build
subtitle: C# 프로젝트 터미널에서 빌드하기 
author: Daniel
categories: C#
tags: 
 - Csharp
 - Programming
 - MacOS
banner:
  image: https://i.imgur.com/InntxOv.jpg
---

C# IDE 가 아닌 Terminal에서 실행하기
--

- IDE에서 빌드는 쉽게 되지만, 실제 콘솔에서 작동하는 환경을 보기 위해 터미널 빌드 실행

### 1. Terminal 프로젝트 접근

```sh
cd 프로젝트 Root 
cd Documents/GitHub/HexCore/TeamDungeon
```

### 2. DotNet Build

- 프로젝트에서 직접 dotnet을 이용해서 프로젝트를 빌드

```sh
프로젝트 루트 $ dotnet build
```

![](https://i.imgur.com/InntxOv.jpg)

- 해당 Warning은 Null 값이 있을 수 있다는 경고문이며, 실제 프로젝트 코드를 수정하면 된다
- 빌드가 성공적으로 수행 되면 해당 메세지를 볼 수 있다

![](https://i.imgur.com/yylWBIE.jpg)

### 3. DotNet 프로젝트.dll

```sh
dotnet 프로젝트/bin/Debug/net7.0/프로젝트.dll

dotnet bin/Debug/net7.0/HexaCoreVillage.dll
```

![](https://i.imgur.com/Wyyct76.jpg)

![](https://i.imgur.com/lns2MIm.jpg)

- 기존에 설정 해 두었던 터미널 dll 사이즈에 맞춰서 빌드가 실행됨

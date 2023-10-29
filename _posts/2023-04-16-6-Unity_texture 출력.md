---
title: Unity Texture 출력
category: Unity
author: 이정훈
tags: [Unity, Object]
img: https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: Unity Texture 출력
---

### 2D 게임 오브젝트 Texture 출력

1. 사용할 이미지 에셋의 Texture Type을 Sprite (2D and UI)로 설정
![](https://i.imgur.com/9uRYVs0.png)

2. GameObject > 2D Object > Sprite 생성
![](https://i.imgur.com/GPVMvtL.png)

3. 생성된 오브젝트의 Sprite Render 컴포넌트에 있는 sprite 변수에 이미지 에셋 등록
![](https://i.imgur.com/kZar8yg.png)


***

### 3D 게임 오브젝트 Texture 출력

1. 사용할 이미지 에셋의 Texture Type을 defualt로 설정
2. Project View에서 Material 에셋 생성
![](https://i.imgur.com/rSjhf6z.png)

3. Material 에셋에 이미지 에셋 등록
![](https://i.imgur.com/cMhIn1J.png)

4. 3D 오브젝트 생성

5. 생성된 오브젝트 컴포넌트의 **Renderer 컴포넌트**에 있는 materials 변수에 Material 에셋 등록
![](https://i.imgur.com/4JgehXV.png)


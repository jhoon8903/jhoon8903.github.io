---
title: Unity Engine
category: unity
author: "이정훈"
tags: [unity, c#]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity Engine"
---

# Unity 3D 기본 용어

## 🔴 프로젝트 (Project)

### 🟢 하나의 게임, 콘텐츠, 어플리케이션
![](https://i.imgur.com/DRs3TTm.png)
***

## 🔴 씬 (Scene)

### 🟢 게임의 장면이나 상태를 저장하는 단위
### 🟢 하나의 거대한 게임을 씬 단위로 관리하며, 코드를 이용해 이동이 가능하다.
	🟡 Intro Scene , Menu Scene , Stage 1 ~ N Scene , Game Over Scene 등

![](https://i.imgur.com/xw6AQxl.png)

🔵 하이어라키 (Hierarchy) 내부에 존재

***

## 🔴 게임 오브젝트 (Game Object)

### 🟢 씬에 배치되는 하나의 물체를 지칭하는 단위
	🟡 모든 게임 오브젝트는 '위치(Position) / 회전(Rotation) / 크기(Scale)'를 제어하는  
		Transform Component를 가지고 있다.
	🟡 게임 오브젝트는 컴포넌트를 묶어서 관리하고, 접근할 수 있는 수단
	
### 🟢 게임 오브젝트에 원하는 컴포넌트를 추가하여 다양한 오브젝트 제작 가능
	🟡 적 오브젝트. 나무 오브젝트, 공격 효과음, 불 이펙트 오브젝트 등

![](https://i.imgur.com/kCw0qGt.png)

🟣 하이어라키에 있는 오브젝트 클릭시 'Inspector'에서 확인이 가능하다.

	🔵 Sprite Renderer : 2D 이미지를 화면에 랜더링
	🔵 Mesh Renderer : 3D 오브젝트를 화면에 출력
	🔵 Audio Source : Audio Clip 변수에 등록된 사운드 에셋을 재생
	🔵 C# Script : 게임 오브젝트에 컴포넌트를 부착하여 게임 오브젝트에 여러 기능을 부여

***

## 🔴 에셋 (Asset)

### 🟢 프로젝트 내부에서 사용하는 모든 리소스를 지칭하는 단위
	🟡 Audio, 3D Model, Animation, Texture, Script, Prefab 등
	
![](https://i.imgur.com/Pi1CEd9.png)

***

## 🔴 프리팹 (Prefab)

### 🟢 하이어라키 뷰에 있는 게임 오브젝트를 파일 형태로 저장하는 단위
### 🟢 주로 게임 중간에 생성되는 게임 오브젝트를 프리팹으로 저장해두고 사용
### 🟢 프리팹의 장점
	🟡 동일한 게임 오브젝트를 여러 Scene이나 게임 월드 특정 장소에 배치할 때 Project View에 
		저장되어 있는 프리팹을 Drag & Drop하여 배치할 수 있다.
	🟡 기획상의 변경이 있을 때 프리팹 원본을 갱신하게 되면, 모든 씬에 복사되어 배치된 게임 오브젝트들도 
		원본과 동일하게 업데이트 된다.

![](https://i.imgur.com/JkqFNwl.png)

🔵 원본을 수정하면 본사본으로 배치되어 있는 모든 게임 오브젝트가 한번에 바뀜

***

## 🟣 Unity 요소 관계도
 
![](https://i.imgur.com/UjrCV5h.png)

![](https://i.imgur.com/qQkJElX.png)
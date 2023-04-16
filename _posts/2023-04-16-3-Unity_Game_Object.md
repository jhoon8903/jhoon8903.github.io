---
title: Unity Game Object
category: unity
author: "이정훈"
tags: [unity]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity Game Object"
---

# Game Object 종류

![](https://i.imgur.com/vVYLVTe.png)
***

### 🔴 Empty Object

![](https://i.imgur.com/TcFKiHM.png)
🟡 GameObject 라는 이름으로 생성됨
🟡 **"Transform"** 컴포넌트는 Positon (이동) / Rotation (회전) / Scale (크기)가 기본 속성

	🟣 주로 게임에는 보이지 않지만, 게임의 여러 요소를 처리하는 오브젝트를 만들 때, Empty Object를 생성
	🟣 게임의 여러 요소를 처리하는 컴포넌트를 추가해서 사용
		🟢 주기적으로 적을 생성하는 적 Spawner, 스테이지 클리어 조건을 관리하는 StageController 등
***

### 🔴 3D Object

![](https://i.imgur.com/XvFcsUg.png)

🟢 3D 게임을 제작할 때 필요한 실제 사람이나 다양한 형태의 모델은 외부 프로그램에서 제작하여 가져온다.
- Unity 3D 는 기본적인 도형 오브젝트만 제공한다.

##### 🟡 Cube

![](https://i.imgur.com/rkSBCXT.png)

🟠 Mesh Filter 컴포넌트 - 3D 오브젝트의 외형
- Mesh 변수에 등록된 데이터에 따라 외형이 드르게 구성된다.
- Cube Mesh는 육면체 모양으로 생김

🟣 Mesh Renderer 컴포넌트 - 3D 오브젝트의 표면 색상
- Material 과 Light 정보를 바탕으로 표면의 질감이 표현됨
- Defualt 는 흰색으로 표현

🟢 Box Collider 컴포넌트 - 게임 오브젝트의 **'충돌'** 범위를 설정
- XX Collider와 같이 앞에 붙는 접두사에 따라 충돌 박사의 외형이 다르다.
- Box Collider는 육면체의 모양

***
### 🔴 2D Object

![](https://i.imgur.com/PQnKS4p.png)


![](https://i.imgur.com/rdlKX6k.png)

🟢 Sprite Renderer 컴포넌트 - 게임 화면에 2D 이미지를 출력
- Sprite 변수에 적용된 Asset(이미지)을 화면에 출력

***
### 🔴 Effect

![](https://i.imgur.com/xnjDj1f.png)

🟢 게임에 사용되는 여러 효과 (불, 번개), 무기의 잔상, 선 그리기와 같은 오브젝트들

***
### 🔴 Audio

![](https://i.imgur.com/NaDOZAS.png)

🟢 게임 내에서 재생되는 사운드와 관현된 오브젝트들

	🟡 소리를 듣는 컴포넌트는 Audio Listener 컴포넌트로 Camera 오브젝트에 부착되어 있다.

***
### 🔴 Video

![](https://i.imgur.com/xMTDjJb.png)

🟢 동영상을 제공하는 오브젝트
***
### 🔴 UI

![](https://i.imgur.com/xirBqBd.png)

🟢 사용자가 게임과 상호작용을 할 수 있는 GUI 오브젝트들
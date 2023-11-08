---
layout: post
title: Unity Light
subtitle: 유니티에서 빛을 담당하는 Light 오브젝트
categories: Unity
author: Daniel
tags: 
 - Unity
 - Light
banner:
 image: https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
---

###  Light 💡
🟨 현실세계의 빛 역할을 담당
- 빛을 이용해 모델의 재질이나 색상을 다양한 형태로 보여주는데 사용된다.  

|빛 이 있을 때|빛 이 없을 때|  
| :---: | :----: |  
|![](https://i.imgur.com/yhzcplD.png)|![](https://i.imgur.com/uKhdj9M.png)

***

### Light 컴포넌트 구성

![](https://i.imgur.com/lKDzfGb.png)

##### 🟢 Type

	🟨 Directional
	- 오브젝트의 방향이 중요한 타입으로 모든 오브젝트에 동일한 방향으로 빛을 제공

	🟩 Point
	- Range (반지름) 변수에 설정된 값 만큼 구(Spear) 
	- 주번을 밝게 만드는 전구, 모닥불 같은 곳에 사용된다.

	🟦 Spot
	- Range(반지름), Spot Angle(원뿔 각도) 변수에 설정된 값 만큼 원뿔 형태로 뻗어나가는 빛
	- 오브젝트를 강조해서 비출 때나 가로등과 같은 곳에서 사용한다.

	 🟧 Area (baked only)
	 - 오브젝트의 위치를 기준으로 전방 방향으로만 방출되는 빛
		 - Range(반지름), 빛의 범위 Width(가로), Height(세로)
	 - Area 는 Baked(빛에 대한 세팅이 먼저 되어 있는 상황) 모드에서만 사용 가능  


|Directional|Point|Spot|Area|  
|:---:|:---:|:---:|:---:|  
|기본설정|![](https://i.imgur.com/PvdqgnH.png)|![](https://i.imgur.com/1tz8GHz.png)|![](https://i.imgur.com/Vk5lVgq.png)|
|![](https://i.imgur.com/RQew80a.png)|![](https://i.imgur.com/jsE4JGd.png)|![](https://i.imgur.com/sjTWZar.png)|![](https://i.imgur.com/1Zmyapn.png)|

***

### 🔴 Mode
- 빛의 연산을 실시간으로 할지 미리 계산해두고 쓸지 결정하는 옵션

	🟧 Realtime
	- 빛이 비추엇을 때의 밝기, 그림자 등의 연산을 실시간으로 함
	- 장점 : 게임 실행 중에도 빛의 이동이나 회전에 따라 환겨이 변화됨
	- 단점 : 연산량이 많아 게임의 최적화에 큰 지자을 줌

	🟨 Mixed
	- Realtime과 Baked를 섞어서 사용

	🟩 Baked
	- 현재 상태로 빛 연산을 해두고 활용
	- 장점 : 연산을 미리 해두고 사용하기 때문에 최적화가 잘 되어있음
	- 단점 : 게임 실행 중에 빛이 이동 / 회전해도 반영되지 않음

🔦  빛이 Baked 모드일 때 설정해야 하는 것들
- 빛의 영향을 받을 게임 오브젝트의 Static 옵션을 설정
![](https://i.imgur.com/Wy29cvh.png)


![](https://i.imgur.com/QtDII72.png)

🔗 Generate Lighting 으로 현재 빛의 속성 생성

![](https://i.imgur.com/q5aoLAf.png)

🔗 Scenes > Subject 에 빛 속성 자동 저장
![](https://i.imgur.com/GM9K0Aw.png)

***

### 🟣 Intensity
- 빛의 세기를 나타내는 수치로 높을 수록 빛의 세기가 커진다.

|Intensity = 1|Intensity = 10|
|:---:|:---:|
|![](https://i.imgur.com/qoKjq19.png)|![](https://i.imgur.com/7nuzfHN.png)|

***

### 🟢 Shadow
- 빛을 받은 오브젝트의 그림자 설정

|No Shadows|Soft Shadows|Hard Shadows|
|:---:|:---:|:---:|
|![](https://i.imgur.com/EWeFHJz.png)|![](https://i.imgur.com/4FCRjvY.png)|![](https://i.imgur.com/ywvl9IE.png)|

***

### 🟡  Cookie
- Cookie에 등록 된 Texture 모양, Cookie Size 크기의 빛 생성

***

### Draw Halo
- 빛 주변으로 후광 효과 생성

### Flare
- 빛의 효과에 대한 모습

### Render Mode
- 중요한 빛과 중요하지 않은 빛을 구분
	- Auto
	- Important
	- Not Impotant

### Culling Mask
- 해당 레이어에 빛을 적용할지 적용하지 않을지 설정
- 선택 해제된 레이어는 해당 빛의 영향을 받지 않는다.
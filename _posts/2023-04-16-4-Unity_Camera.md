---
layout: post
title: Unity Camera
subtitle: Camera Object
categories: Unity
author: Daniel
tags: 
 - Unity
 - Camera
banner:
 image: https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
---

### 🔴 Unity Camera 📸 

	🟢 카메라 오브젝트는 씬에 최소한 1개 이상이 존재해야 함
	- 여러개의 카메라를 이용해 CCTV와 같이 분할된 화면을 관찰하는 것도 가능
	- 카메라의 이동 or 회전 애니메이션을 추가해 장면의 연출 효과를 주기도 함

![](https://i.imgur.com/7hYEcCo.png)

🟢 Camera 컴포넌트 - 게임 월드를 보는 **'눈'** 의 역할을 담당
🟠 Audio Listener 컴포넌트 - 컴포넌트가 내는 소리를 듣는 역할


![](https://i.imgur.com/WIce7Gr.png)

##### 🟠 Clear Flags - 오브젝트가 존재하지 않는 배경을 어떻게 채울지 정하는 요소
	- 주로 2D 게임을 Solld Color, 3D 게임은 Skybox 를 사용
	- 카메라를 동시에 여러 대 사용할 때 Depth only, Don't Clear 옵션도 사용

##### 🟠 Background - Clear Flags 가 'Solid Color'일 때 배경색상을 나타내는 변수

Skybox (3D)  

|Clear Flags : Skybox|Clear Flags : Solid Color|  
|:---:|:---:|  
|![](https://i.imgur.com/ETiRkJA.png)|![](https://i.imgur.com/OIUzbuk.png)|

***

##### 🟣 Projection - 카메라의 시점을 나타내며, 2D와 3D 시점이 존재한다.

|2D MODE : Orthographic|3D MODE : Perspetive|  
|:---:|:---:|  
|![](https://i.imgur.com/m5yR6sZ.png)|![](https://i.imgur.com/lH6zhn2.png)|  
|Size : 카메라의 시야 범위|Field of View : 카메라의 시야범위<br>FOV Axis: 시야 범위를 넓히는 방향<br>Vertical, Horizontal|  
|![](https://i.imgur.com/Z6ANGgD.png)|![](https://i.imgur.com/VdjY5p2.png)
|Z축은 어떤 오브젝트가<br>더 앞에 그려질지에만영향을 줌|멀리 떨어져 있는 오브젝트는 <br>더 작게 보이는 원근 투영 적용|

***

##### 🔵 Clipping Planes - 카메라 오브젝트를 볼 수 있는 시야 거리
- Near는 시작지점 (defualt 0.3), Far는 끝 지점 (defualt 1000)
- Far 거리를 느리면 카메라가 볼 수 있는 시야 거리가 늘어난다.
	- 단, 시야가 늘어나는 만큰 그리는 오브젝트 수가 많아져 성능에 영향을 미침
	- ⚪️ 현실에서도 눈과 같은 위치에 있는 오브젝트는 보이지 않는 것 처럼,  
		  Near 가 0.3이기 때문에 카메라 오브젝트로 부터 0.3 떨어진 게임 오브젝트부터 보임

##### 🔵 Viewport Reat - 카메라가 본 것을 화면에 출력하는 영역
- Min 0 / Max 1
- X, Y 는 출력을 시작하는 위치좌표
- W, H 는 출력하는 화면 크기

![](https://i.imgur.com/DPQvHFu.png)

***

##### 🟥 Rendering Path - 카메라에 그려지는 방법
##### 🟧 Occlusion Culling - 오브젝트 뒤에 오브젝트가 배치되어 카메라의 시선에 보이지 않을 경우 &ensp;&ensp;&ensp;해당 오브젝트를 그리지 않도록 해 게임의 처리속도를 높여주는 기능
##### 🟨 HDR - 눈부심, 글로우 같은 조명 효과
##### 🟩 Allow Dynamic Resolution - 동적 해상도 지원 여부
##### 🟦 Target Display - 카메라가 최대  8개 서로 다른 모니터에 카메라를 출력할 수 있음

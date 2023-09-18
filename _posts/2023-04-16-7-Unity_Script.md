---
title: Unity C# Script
category: Unity
author: 이정훈
tags:
  - unity
img: https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: Unity C# Script
---

# Unity C# Script

### 🔴 스크립트의 역할
- 스크립트가 컴포넌트로 종속된 게임 오브젝트에 주어지는 각종 명령 제어
- 게임 내에 사용되는 여러 오브젝트들을 생성, 삭제 및 관리
- 게임 전체 또는 일부를 관리하는 게임 내 시스템 구현

![](https://i.imgur.com/2HihK1k.png)


![](https://i.imgur.com/q9flose.png)

***

### 🔴 Unity Event Function(  )

![](https://i.imgur.com/dwEsPxV.png)


![](https://i.imgur.com/PqRkduX.jpg)


![](https://i.imgur.com/nMVo299.jpg)

##### 초기화 이벤트 함수
- Awake (  )
	- 현재 Scene에서 게임오브젝트가 활성화 되어 있을 때 1회 호출
	- 비활성화 되어 있다면 활성화 할때 1회 실행
	- 데이터를 초기화 하는 용도로 사용

- OnEnable (  )
	- 컴포넌트가 비활성와 되었다가 **활성화 될 때마다 1회 실행**

- Start (  )
	- 현재 Scene에서 게임오브젝트와 컴포넌트 **모두** 활성화 되었을 때 1회 호출
	- 데이터를 초기화 하는 용도로 사용
	- 첫 번째 업데이트 함수가 실행되기 직전에 호출
	- Awake() -> OnEnable() -> Start()


##### 업데이트 이벤트 함수
- Update (  )
	- 현재 Scene이 실행 된 후 컴포넌트가 활성화되어 있을 때 **매 Frame**마다 호출
	- FPS 60이라면, Update() 1초에 60번 호출 됨

- LateUpdate (  )
	- 현재 Scene에 **존재하는 모든 게임오브젝트의 Update() 1회 실행 된 후 실행**
	- Update() -> LateUpdate()
	- 플레이어 캐릭터가 Update()를 이용해 움직이고 난 후 카메라는 LateUpdate()에서 플레이어의 위치를 바탕으로 이동을 한다.
	- Update()와 마찬가지로 FPS에 영향을 받음

- FixedUpdate (  )
	- frame에 영향을 받지 않고 일정한 간격으로 호출
	- Edit > Project Setting > Time 옵션의 Fixed Timestep 변수로 조절 가능
	- 기본값 0.02로 0.02초에 1번 호출 = 1초에 50번 호출
![](https://i.imgur.com/aspBk3T.png)


##### 오브젝트 파괴 이벤트 함수
- OnDestroy(  )
	- 게임 오브젝트가 파괴될 때 1회 호출
	- Scene이 변경 되거나 게임이 종료될 때에도 오브젝트가 파괴되기 때문에 호출된다.


##### 종료 이벤트 함수
- OnApplicationQuit(  )
	- 게임이 종료될 때 1회 호출
	- 유니티 에디터에서는 플레이 모드를 중지할 때 호출된다.

- Ondisable(  )
	- 컴포넌트가 활성화 되었다가 비활성화 될 때 마다 1회 호출 (OnEnable() 과 반대)
***

### 유니티 사전 정의 이벤트 함수 Flow

![](https://i.imgur.com/d3dteBF.png)

![](https://docs.unity3d.com/uploads/Main/monobehaviour_flowchart.svg)

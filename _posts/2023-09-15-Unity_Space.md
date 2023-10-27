---
title: Utniy_Space 공간
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/NCdrbmb.gif
comments_disable: true
meta_description: Utniy_Space 공간
---
# Unity Coordinate System

- 3D 공간에 배치하는 3D 오브젝트는 좌표(Coordinate)를 가짐
- 원점 Vector3 (0,0,0) 을 기준
- x => 오른쪽
- y => 위쪽
- z => 앞쪽
- 전역공간, 오브젝트 공간, 자식 공간 3가지의 위치속성이 있음

---

## 전역공간 Global Space
- World의 중심이라는 절대 기준이 있는 공간 (World Space)
- 전역 좌표계(Global Coordinate Space)를 기준으로 함
- 모든 오브젝트가 원점 (0,0,0)에서 얼마나 떨어져 있는지가 오브젝트의 좌표가 됨
![200](https://i.imgur.com/wN0hC7h.png)

### Global / Local (Y Rotation 60)

|Global|Local|
|:--:|:--:|
|![](https://i.imgur.com/ImRJx38.png)|![](https://i.imgur.com/D1weATP.png)|
- 큐브의 회전을 무시하고 Global 기준으로 배치
- Global Mode에서 큐브를 z로 이동시키면 '큐브의 z'가 아닌 'Global' z의 방향으로 움직임
![](https://i.imgur.com/NCdrbmb.gif)

---
## 오브젝트 공간 Local Space
- 오브젝트 자신의 (x,y,z) 방향으로 움직임 (오브젝트 좌표계)
- 오브젝트의 'z'는 World의 'z'와 다를 수도 있음

|Cube z 이동|Inspector 변화|
|:---:|:---:|
|![](https://i.imgur.com/wOzwroA.gif)|![](https://i.imgur.com/NKfSeaX.gif)|
- 큐브의 Y Rotation이 90이 되면, `World Space의 X` 와 `Local Cube Object의 Z`가 일치
- 오브젝트의 앞(Z)는 World의 오른쪽(X)

---

## 자식 공간
- 자식 공간은 하이어라키 상에 자신의 부모 오브젝트를 기준으로 설정

|Cube의 자식 오브젝트 Capsule|
|:---:|
|![200](https://i.imgur.com/GvVIntR.png)|

|부모(Local)|자식 (자식 공간)|
|:--:|:--:|
|![](https://i.imgur.com/VPyVCsS.png)|![](https://i.imgur.com/fTPKEhe.png)|


|부모 자식 관계 해제 후|
|:--:|
|![](https://i.imgur.com/esv7vu6.png)|

|Cube(Local)|Capsule(Local)|
|:---:|:--:|
|![](https://i.imgur.com/viGlykH.png)|![](https://i.imgur.com/geG8ZCL.png)|
- Capsule(자식)의 위치는 부모의 위치만을 확인 함











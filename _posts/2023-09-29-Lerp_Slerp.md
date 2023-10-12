---
title: Unity_Vector3.Lerp / Vector3.Slerp
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/l68H2Vt.jpg
comments_disable: true
meta_description: Unity_Vector3.Lerp / Vector3.Slerp
---
# 선형보간 / 구면 선형보간

- 선형 보간 (Liner Interpolation)
- 구면 선형 보간 (Spherical Linear Interpolation)
- 시작점과 끝점 사이의 특정 위치의 값을 추정할 때 사용
- 보간 함수는 현재 못푯값으로 변경할 때 갑자기 변경하지 않고 부드럽게 변경시키는 로직에 많이 활용
```csharp
Vector3.Lerp(시작 좌표, 종료 좌표, t);
Mathf.Lerp(시작 값, 종료 값, t);
Quaternion.Lerp(시작 각도, 종료 각도, t);
```

###  선형보간 차트 (Liner Interpolation)
![](https://i.imgur.com/l68H2Vt.jpg)
[2021_유니티 절대강좌_주인공 케릭터 제작]

- 빨간 점 이 시작이고 파란 점이 끝점으로, 시작점에서 끝점으로 이동하는 데 걸리는 시간 t를 1초로 가정시
- 0.25초 일 때의 이동 거리와 0.5초일 때의 이동 거리가 동일하다.
```csharp
Mathf.Lerp(0,10,0.0f); // 0을 반환
Mathf.Lerp(0,10,0.5f); // 5을 반환
Mathf.Lerp(0,10,0.8f); // 8을 반환
Mathf.Lerp(0,10,1.0f); // 10을 반환
```

### 구면 선형 보간 차트 (Spherical Linear Interpolation)
![](https://i.imgur.com/y6GuUrC.jpg)
[2021_유니티 절대강좌_주인공 케릭터 제작]

- 직선 형태가 아닌 구면(구체)형태로 값을 추론
- 구면을 따라서 값을 반환하기에 시간 t가 증가할 때 시작점과 종료점은 느리게 증가
- 중간지점은 동일한 시간 대비 이동해야 할 거리가 크기 때문에 빠르게 이동하는 특성이 있다.
- 주로 회전 로직에 사용
```csharp
Vector3.Slerp(시작 좌표, 종료 좌표, t);
Quaternion.Slerp(시작 좌표, 종료 각도, t);
```

# Vector3.Smoothdamp
- 보통 카메라의 Follow 로직에 사용
```csharp
Vector3.SmoothDamp(Vector3 current, 
				   Vector3 target, 
				   ref Vector3 currentVelocity,
				   float smoothTime,
				   float maxSpeed,
				   float deltaTime);
```

|매개변수|설명|
|:--:|:--|
|current|시작위치|
|target|목표위치|
|currentVelocity|현재속도|
|smoothTime|목표 위치까지의 도달 시간|
|maxSpeed|최대 속력 제한 값(기본값 : 무한대 Mathf.infinity), 생략가능|
|deltaTime|프레임 보정을 위한 델타 타임(기본값 : Time.deltaTime), 생략가능|


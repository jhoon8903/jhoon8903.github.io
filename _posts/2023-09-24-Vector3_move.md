---
layout: post
title: Unity Vector 이동
subtitle: 유니티에서 Vector를 다루는 방법
categories: Unity
author: Daniel
tags: 
 - Unity
 - Vector
 - Tranform
banner:
 image: https://i.imgur.com/SfgRWIJ.png
---


Struct Vector3
--

|속성|설명|
|:--:|:--|
|magnitude|벡터의 길이 (Read Only)|
|normalized|크기가 1인 벡터, 정규화 벡터 (Read Only)|
|sqrMagnitude|벡터의 길이의 제곱 (Read Only)|
|x|벡터의 x 성분 (3D 공간의 x 좌표)|
|y|벡터의 y 성분 (3D 공간의 y 좌표)|
|z|벡터의 z 성분 (3D 공간의 z 좌표)|


## 정규화 벡터 (normalized Vector)
- 벡터는 크기와 방향을 나타낼 수 있는 데이터 타입으로, 그중 각 축의 크기가 **1인 벡터**
- Normalized Vector || Unit Vector 라고 함
- 즉, **방향**만 표시하는 벡터

|정규화 벡터 메소드|벡터 좌표|
|:--:|:--|
|⭐️ Vector3.forward|Vector3(0,0,1)|
|Vector3.back|Vector3(0,0,-1)|
|Vector3.left|Vector3(-1,0,0)|
|⭐️ Vector3.right|Vector3(1,0,0)|
|⭐️ Vector3.up|Vector3(0,1,0)|
|Vector3.down|Vector3(0,-1,0)|
|⭐️ Vector3.one|Vector3(1,1,1)|
|⭐️ Vector3.zero|Vector3(0,0,0)|

🌟 유니티에서는 Vector의 평가를 왼손 좌표계를 사용하기 때문에 Vector3.forward 의 경우 z축(앞으로) 이동

![](https://i.imgur.com/kH49xwb.png)

## Translate

게임 오브젝트의 이동 처리를 편하게 할 수 있는 함수
```csharp
void Translate(Vector3 direction, [Space relativeTo])
```

- 두번째 인자 `[Space relativeTo]`는 게임오브젝트의 월드 좌표 (Space.World)기준으로 이동할 지 
- 로컬 좌표 (Space.Self)를 기준으로 할지 결정 
- 생략시 Local 좌표 기준으로 이동 
참조 [[2023-09-15-Unity_Space]]

## Time.deltaTime

- 이전 프레임의 시작 시각부터 현재 프레임이 시작되는 시각의 차 (delta)를 뜻함
- 이전 프레임부터 현재 프레임까지 걸린 시간의 차

```csharp
playerTr.Translate(Vector3.forward * (Time.deltaTime * 1.0f), Space.Self);
```

⭐️ 30 fps
- 1 frame 마다 1 유닛을 이동한다고 하였을때 Update 함수에서 실행되는 횟수는 30회
- 구동 환경의 Time.deltaTime은 1 / 30 (0.033/s)

⭐️60 fps
- 1 frame 마다 1유닛을 이동한다고 하였을 때 Update 함수에서 실행되는 횟수는 60회
- 구동 환경의 Time.deltaTime은 1 / 60 

🌟 Update또는 LateUpdate 등에 이동 및 회전에 대한 설정이 들어가면 Time.deltaTime을 사용해야 한다.
```csharp
// 프레임마다 10 유닛씩 이동 (구동 환경에 따라 달라짐)
transform.Translate(Vector3.forward * 10)

// 매 초 10 유닛씩 이동 (구동 환경에 상관 없이 일정한 속도를 유지)
transform.Translate(Vector3.forward * Time.deltaTime * 10)
```

🌟 정규화를 통해 방향만을 추출하지 않으면 대각선 이동 시에 속도가 증가하는 문제가 발생함
```csharp
private void Update()  
{  
	var horizontalMove = Input.GetAxis("Horizontal"); // -1.0f ~ 0.0f ~ +1.0f;  
	var verticalMove = Input.GetAxis("Vertical"); // -1.0f ~ 0.0f ~ +1.0f;  

	var moveDirection 
		= (Vector3.forward * verticalMove) + (Vector3.right * horizontalMove);  
  
// Transform 컴포넌트의 position 속성값을 변경 (normalized)// playerTr.position += Vector3.forward * 1;  
// Translate 함수 적용 (로컬 좌표 이동 시 Space.Self 생략 가능)  
// playerTr.Translate(Vector3.forward * 1.0f, Space.Self);  
// deltaTime적용으로 frame rate에 영향을 받지 않음  
// 정규화를 통해 방향성만 추출하여 이동 속도를 유지 할 수 있도록 해야 함  
// 정규화를 사용하지 않을 시 피타고라스 정리에 의하여 속도가 1.4이상이 나오게 됨  
	playerTr.Translate(
		moveDirection.normalized * (Time.deltaTime * moveSpeed), Space.Self);  
}
```

```csharp
privatw void Start()
{
	var vector1 = Vector3.Magnitude(Vector3.forward);
	var vector2 = Vector3.Magnitude(Vector3.forward + Vector3.right);
	var vector3 = Vector3.Magnitude(Vector3.forward + Vector3.right).normalized;

	Debug.Log(vector1);  / 1
	Debug.Log(vector2);  / 1.414214
	Debug.Log(vector3);  / 1
}
```


---
layout: post
title: Cinemachine으로 케릭터 Follow 하기
subtitle: Cinemachine 으로 케릭터 이동 추적하기
author: Daniel
categories: Unity
tags: 
 - Project
 - Camera
 - Cinemachine
 - Unity
banner:
  image: https://i.imgur.com/wt52wWu.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)
 
 
 Cinemachine
 --

2020 버전 이후 나온 Cinemachine 은 가상의 카메라를 설정하여 사용자가 보여주고 싶은 화면을 자연스럽게 또는 멋지게 연출할 수 있는 기능으로 다양한 영상 연출에 사용되는 에셋입니다.

여기서는 별도의 코딩 없이 가상카메라가 플레이어를 따라다니는 Follow Cam을 만들예정입니다.

### 1. Asset 설치

- Package Manager에서 Unity Registry 카테고리에서 `Cinemachine`을 설치

![](https://i.imgur.com/Of956n8.jpg)

### 2. Virtual Camera 생성

- 하이어라키상에서 => Cinemachine => Virtual Camera를 생성합니다.

![](https://i.imgur.com/V4r0nHs.jpg)

### 3. Virtual Camera Inspector

- 정말 다양한 기능이 있지만 여기선 목적에 맞는 카메라 Follow  기능만을 소개합니다.

![](https://i.imgur.com/InFyNae.jpg)

#### Follow
- 카메라가 Follow 할 오브젝트를 할당하면 해당 오브젝트를 카메라가 자동으로 따라갑니다.

#### Loot At
- 카메라가 Game 화면에 보여줄 오브젝트를 할당하면 해당 화면을 비춥니다.

#### lens Vertical FOV
- 화각을 설정하면 숫자가 작으면 좁은 각을 커지면 커질수록 넓은 화면을 보여줍니다.

#### Body
- 카메라의 설정이며, 다양한 기능을 제공하지만 여기서는 플랫한 카메라 무빙을 보여주는 `Transposer`을 사용합니다.

#### Aim
- 카메라의 촛점 즉 어디를 중앙으로 비추는지를 선택하며 `Hard Look At`으로 하면 Follow하는 오브젝트를 계속해서 고정되어 따라갑니다.

- 추가적인 정보는 Unity 공식 [Docs](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.2/manual/CinemachineVirtualCamera.html)를 확인하시면 됩니다.

> 이렇게 간단하게 세팅을 하게 되면 별도의 코딩 없이 카메라는 항상 케릭터를 Follow 하게 됩니다.


![](https://i.imgur.com/qTnotkZ.gif)


### 문제점 

- 원래 코드는 Main Camera의 World position을 값으로 받아 케릭터를 회전 하였는데 Virtual Camera를 사용하면 서 해당 WorldPosition의 값이 정상적으로 들어오지 않아 일부 코드의 수정이 필요

```csharp
/// <summary>  
/// 회전 관련 message를 수신하여 이벤트를 호출하는 메서드  
/// </summary>  
/// <param name="value"></param>  
public void OnLookForward(InputValue value)  
{  
	// 매개변수로 받은 벨류값에서 Vector2를 찾음  
	Vector2 lookValue = value.Get<Vector2>();  
	  
	// 마우스 좌표는 카메라의 좌표와는 다르기 때문에 혀재 스크린의 좌표로 변환  

	// 수정 전
	// Vector2 worldPos = _camera.ScreenToWorldPoint(lookValue); 

	// 수정 후
	Vector2 worldPos = _camera.ScreenToWorldPoint(new Vector3(lookValue.x, lookValue.y, _camera.nearClipPlane));  
	// 카메라의 값과, 실제 오브젝트의 vector 값을 비교하여 정규화 하여 보장된 상대적 좌표 값을 반환  
	Vector2 relativeLookValue = (worldPos - (Vector2)transform.position).normalized;  
	  
	if (relativeLookValue.magnitude >= 0.9f)  
	{  
	// 이벤트 호출  
		CallLookForwardEvent(relativeLookValue);  
	}  
}
```

### 두 코드의 차이점 

```csharp
Vector2 worldPos = _camera.ScreenToWorldPoint(lookValue); 

Vector2 worldPos = _camera.ScreenToWorldPoint(new Vector3(lookValue.x, lookValue.y, _camera.nearClipPlane));  
```

#### **수정 전**

- `lookValue`는 `Vector2`입니다. 즉, x 및 y 구성요소만 있음을 의미합니다.
- `lookValue`가 `ScreenToWorldPoint`에 전달되면 메서드는 내부적으로 z 값을 가정합니다. 
- Unity에서 'Vector3'이 예상되는 곳에 'Vector2'를 사용하면 z 값이 자동으로 0으로 설정됩니다.
- 이 0 z 값은 카메라의 근거리 클리핑 평면에 해당합니다. 
- 그러나 이것이 카메라가 바라보는 위치를 기준으로 항상 올바른 월드 위치를 제공하는 것은 아니라는 점에 유의하는 것이 중요합니다. 
- 이는 단순히 카메라 바로 앞, 가까운 클리핑 평면에 있는 화면 지점을 월드 공간으로 변환합니다.

#### **수정 후**
    
- `lookValue`는 명시적으로 `Vector3`으로 변환되며 z 값은 카메라의 근거리 클리핑 평면(`_camera.nearClipPlane`)으로 설정됩니다.
- z 값을 카메라의 가까운 클리핑 평면으로 설정하면 스크린 포인트가 카메라로부터 올바른 거리에 있는 월드 포인트로 변환됩니다. 
- 이는 특히 깊이가 중요한 역할을 하는 투시 카메라를 다룰 때 3D 공간에서 정확한 위치를 지정하는 데 중요합니다.
- 이 접근 방식은 일반적으로 더 정확하며 카메라 뷰의 화면 위치를 월드 위치에 매핑하려는 경우 화면 포인트를 월드 포인트로 변환하는 데 권장되는 방법입니다.


요약하자면, 코드의 첫 번째 줄은 항상 정확한 세계 위치를 산출하지 못할 수도 있는 z 좌표에 대해 암시적인 가정을 합니다. 대조적으로, 코드의 두 번째 줄은 z 좌표를 카메라의 가까운 클리핑 평면으로 명시적으로 설정하여 화면 공간에서 월드 공간으로 더 정확하게 변환합니다.


![](https://i.imgur.com/wt52wWu.gif)

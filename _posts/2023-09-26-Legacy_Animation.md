---
title: Unity Legacy Animation
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/rtTIPZs.png
comments_disable: true
meta_description: Unity Legacy Animation
---

# Animation 
- Legacy Animation : 하위 호환성을 고려한 애니메이션, 소스코드로 컨트롤 해야 함
- Mecanim Animation : 모션 캡처 애니메이션, 리타겟팅 기능

## Animation Type
![](https://i.imgur.com/rtTIPZs.png)

|Animation Type 옵션|설명|
|:--:|:--|
|None|애니메이션을 사용하지 않는다.|
|Legacy|하위 호환성을 유지하기 위한 이전 방식의 애니메이션|
|Generic|메카님 애니메이션, 인체형 모델이 아닌 3D 모델에 적용. 리타게팅 불가|
|Humanoid|메카님 애니메이션, 리타게팅이 가능하며, 사람과 같이 2족 보행하는 모델에 적용|

 💫 애니메이션 타입에 따라 컴포넌트도 바뀌어야 한다.
 - Legacy => Animation 컴포넌트
 - Mecanim => Animator 컴포넌트

## Animation Clip
- 케릭터의 걷기, 달리기, 점프, 총 쏘기 등과 같은 동작을 기록한 파일
- 애니메이션 컴포넌트는 애니메이션 클립에 기록 된 Rig 위치나 Rotate 값을 프레임 단위로 재생시킨다.

### 3D 모델링 툴 제작방법
#### 1. 모든 애니메이션 클립이 하나의 애니메이션 파일에 들어 있는 경우
- 각 애니메이션 클립이 시작 프레임과 종료 프레임을 가지는 방식
- 개발자가 직접 분리하여 사용하여야 함
#### 2. 1번 과 같지만 분리 되어 있는 경우

#### 3. 애니메이션 클립을 동작별로 분리해 별도 파일로 생성하는 경우
- "모델명@애니메이션 클립명"형태의 명명 규칙 적용

## Animation Blending

- 현재 수행 중인 애니메이션에서 다른 애니메이션으로 변경될 때 이를 부드럽게 연결해주는 기능
- 급격한 애니메이션 변화로 인한 어색함을 방지하기 위해 CrossFade 함수 제공
- CrossFade 의 경우 첫번째 인자는 'Animation Clip', 두 번째 인자는 'Fade Out Time'

```csharp
private void PlayerAnimation(float h, float v)  
{  
	// KeyBoard Input Value를 기준으로 동작할 애니메이션 수행  
	switch (v)  
	{  
		case >= 0.1f:  
			// Forward Animation 실행  
			_playerAnimation.CrossFade("RunF", 0.25f);  
			break;  
		case <= -0.1f:  
			// Back Animation 실행  
			_playerAnimation.CrossFade("RunB", 0.25f);  
			break;  
		default:  
		{  
			switch (h)  
			{  
				case >= 0.1f:  
					// Right Animation 실행  
					_playerAnimation.CrossFade("RunR", 0.25f);  
					break;  
				case <= -0.1f:  
					// Left Animation 실행  
					_playerAnimation.CrossFade("RunL", 0.25f);  
					break;  
				default:  
					_playerAnimation.CrossFade("Idle", 0.25f);  
					break;  
			}  
			break;  
		}  
	}  
}
```

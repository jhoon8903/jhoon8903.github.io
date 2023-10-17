---
title: Unity Input System
category: Unity
author: 이정훈
tags:
  - unity
img: 
comments_disable: true
meta_description: Unity Input System
---
# Legacy Input System


## Input System 구조

```csharp
private void Update()
{
	// 키보드 입력값
	var h = Input.GetAxis("Horizontal");
	var v = Input.GetAxis("Vertical");

	var moveDir = (Vector3.forward * v) + (Vector3.right * h);
	transform.Translate(moveDir.normalize * Time.deltaTime * moveSpeed);

	// 점프 로직
	if (Input.GetKey(KeyCode.Space))
	{
		// Space 키 입력시 점프 로직 실행
	}
}
```

- 다양한 입력장치로의 전환이 힘듬 (점프를 실행하려면 Space Key가 입력 되어야 함)
- 모바일, 터치스크린 등에서 실행 불가
- Update는 프레임마다 호출 되기 떄문에 프레임마다 입력값을 확인 해야 함 (성능하락)

## New Input System
### 설치
- Pakage Manager를 통해 `Input System`설치 가능
![](https://i.imgur.com/iKrtVaa.jpg)

- Edit => ProjectSetting => Player 
  => OtherSetting => Active Input Handling => Input System Package (new) 선택

![](https://i.imgur.com/14rmszi.jpg)

### 구조
- Input Actions Asset을 생성해 각종 입력값을 정의하고 할당
- Action Maps / Actions / Properties 로 구성 됨
![](https://i.imgur.com/LGo7prn.jpg)
#### Action 
- 게임 내의 행동, 동작을 의미
- 예를 들면 이동, 회전, 점프, 공격, 방어, 패링 등이 모두 Action
#### Binding
- Action에 물리적인 입력장치와 매핑하는 것

![](https://i.imgur.com/a8X67jG.jpg)

### Input Action 개념도
![](https://i.imgur.com/ofOWfwX.jpg)


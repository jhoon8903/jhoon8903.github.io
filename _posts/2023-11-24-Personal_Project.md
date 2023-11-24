---
layout: post
title: 케릭터 이동 및 로테이션
subtitle: Input System을 이용한 이동 및 회전
author: Daniel
categories: C#
tags: 
 - Unity
 - InputSystem
 - Project
banner:
  image: https://i.imgur.com/ebjRg1J.gif
---

![](https://i.imgur.com/ebjRg1J.gif)

Unity 프로젝트 시작
--

오랜만에 Unity를 이용한 프로젝트를 제작
새로운 Input System을 이용한 방법으로 케릭터 이동 및 회전을 구현하는 것이 첫번째 과제

## Input System

기본적인 Input System은 다음 링크에 정리 [Unity Input System](https://jhoon8903.github.io/unity/2023/10/17/4-Input-System.html)

- 입력값은 키보드의 W,S,A,D 와 ↑,↓,←,→ 키를 이용하여 입력을 바인딩한다

![](https://i.imgur.com/HwkAtwa.jpg)

- 마우스의 입력값은 마우스의 Position 을 값으로 받아온다

![](https://i.imgur.com/jb0QJTi.jpg)


### Send Message

- Move 와 LookForward 의 경우 Event Action 을 이용하여 Call back으로 입력되는 입력 값을 처리하여 해당 메서드를 호출 하는 `Message` 방식을 사용
- 해당 Action이 발생하면 앞에서 Input System에서 바인딩한 Action인 `Move`와 `LookForward` 앞에 **`On`** 이 붙는 이벤트에 `message`전달이 가능해진다

![](https://i.imgur.com/xoWoSB2.jpg)


### Event Controller

- 전달 받은 이벤트를 실행 시킬 수 있으며 **`Player Controller`** 의 부모 스크립트로 이벤트를 상속해줄 Event Controller를 먼저 작성하여 이벤트를 생성 및 할당 합니다.

```csharp
using System;  
using UnityEngine;  
  
namespace Script.Controller  
{  
	public class MovementEventController : MonoBehaviour  
	{  
		public event Action<Vector2> MoveEvent; // Move Event Action  
		public event Action<Vector2> LookForwardEvent; // Rotate Event Action  
		  
		/// <summary>  
		/// 케릭터가 움직일 때 호출 되는 Move Event 메서드  
		/// </summary>  
		/// <param name="value">2D 평면 이동 Vector2 값</param>  
		protected void CallMoveEvent(Vector2 value)  
		{  
			MoveEvent?.Invoke(value);  
		}  
		  
		/// <summary>  
		/// 케리터가 이도할때 마우스 또는 입력값에 따라 좌우 방향으로 바라보는 이벤트 메서드  
		/// </summary>  
		/// <param name="value">좌 우 를 기준으로 바뀌는 Vector2 값</param>  
		protected void CallLookForwardEvent(Vector2 value)  
		{  
			LookForwardEvent?.Invoke(value);  
		}  
	}  
}
```

- Event Action의 뒤에 붙은 `?` 는 Nullable 로 Null 이 아닐때만 작동 할 수 있도록 Null 사용을 명시해 불필요한 이벤트 호출을 방지 합니다.


### Player Controller

- 다음은 메세지를 전달 받고 작동하게 되는 메서드를 Player Controller에 만들어 줍니다.
- Event Controller를 상속 받으며, Input system의 `Message`를 전달 받아 이벤트에 넘겨줄 메소드를 생성합니다.

```csharp
using Unity.VisualScripting;  
using UnityEngine;  
using UnityEngine.InputSystem;  
  
namespace Script.Controller  
{  
	public class PlayerController : MovementEventController  
	{  
		private Camera _camera;  
		  
		/// <summary>  
		/// 스크립트가 처음으로  
		/// 메인 카메라를 하이어라키에서 찾아 할당하는 메서드  
		/// </summary>  
		private void Awake()  
		{  
			_camera = Camera.main;  
		}  
		  
		/// <summary>  
		/// 이동 관련 message를 수신하여 이벤트를 호출하는 메서드  
		/// </summary>  
		/// <param name="value"></param>  
		public void OnMove(InputValue value)  
		{  
			// 매개변수로 전달받은 'value'에서 Vector2 값을 찾아 정규화 하여  
			// 대각선에 대해서 일정한 속도를 유지 할 수 있도록 보정하는 코드  
			Vector2 moveValue = value.Get<Vector2>().normalized;  
		  
			// 보정된 Vector2 값을 호출하는 이벤트 메서드  
			CallMoveEvent(moveValue);  
		}  
		  
		/// <summary>  
		/// 회전 관련 message를 수신하여 이벤트를 호출하는 메서드  
		/// </summary>  
		/// <param name="value"></param>  
		public void OnLookForward(InputValue value)  
		{  
			// 매개변수로 받은 벨류값에서 Vector2를 찾음  
			Vector2 lookValue = value.Get<Vector2>();  
		  
			// 마우스 좌표는 카메라의 좌표와는 다르기 때문에 혀재 스크린의 좌표로 변환  
			Vector2 worldPos = _camera.ScreenToWorldPoint(lookValue);  
		  
			// 카메라의 값과, 실제 오브젝트의 vector 값을 비교하여 정규화 하여 보장된 상대적 좌표 값을 반환  
			Vector2 relativeLookValue = (worldPos - (Vector2)transform.position).normalized;  
		  
			if (relativeLookValue.magnitude >= 0.9f)  
			{  
				// 이벤트 호출  
				CallLookForwardEvent(relativeLookValue);  
			}  
		}  
	}  
}
```

- OnMove, OnLookForward 에서 `InputValue`로 받은 Param Value를 이벤트 호출자에 넘겨 이벤트를 실행시켜줍니다.
- _camera 변수의 경우는 마우스와 스크린 사이의 상대적인 Vector값을 구하기 위해 `ScreenToWorldPosition`을 사용해 WorldPosition 값을 가지고 옵니다.
- `value.Get<Vector2>`의 경우는 Input system에서 바인딩할때 사용한 value 값인 Vector2 값을 가져와 변수에 할당합니다.

![](https://i.imgur.com/CBIR9hI.jpg)



### Player Action

- 발생한 이벤트를 구독하여 이벤트 호출에 맞추어 케릭터 오브젝트를 움직이게 하는 스크립트
- 물체를 움직이기 위해서는 RigidBody2D를 같이 컴포넌트에 추가하여 물리적 운동을 재현
- LateUpdate를 사용하여 고정적인 프레임을 계속해서 실행하며, 케릭터의 입력을 프레임 단위로 확인하여 움직임을 실행

```csharp
using System;  
using Script.Controller;  
using UnityEngine;  
  
namespace Script.ActionScript  
{  
	public class CharacterAction : MonoBehaviour  
	{  
	  
		private MovementEventController _movementEventController;  
		  
		#region Move Variables  
		[SerializeField] private float speed;  
		private Vector2 _moveDirection;  
		private Rigidbody2D _rigidbody2D;  
		#endregion  
		  
		#region LookForward Variables  
		[SerializeField] private SpriteRenderer character;  
		#endregion  
		  
		private void Awake()  
		{  
			_movementEventController = GetComponent<MovementEventController>();  
			_rigidbody2D = GetComponent<Rigidbody2D>();  
		}  
		  
		private void Start()  
		{  
			_movementEventController.MoveEvent += Move;  
			_movementEventController.LookForwardEvent += LookForward;  
		}  
		  
		private void LateUpdate()  
		{  
			MoveToward(_moveDirection != Vector2.zero ? _moveDirection : Vector2.zero);  
		}  
		  
		private void Move(Vector2 moveDirection)  
		{  
			_moveDirection = moveDirection;  
		}  
		  
		private void LookForward(Vector2 lookDirection)  
		{  
			float rotate = Mathf.Atan2(lookDirection.y, lookDirection.x) * Mathf.Rad2Deg;  
			character.flipX = Mathf.Abs(rotate) > 90f;  
		}  
		  
		private void MoveToward(Vector2 moveDirection)  
		{  
			Vector2 direction = moveDirection * speed;  
			_rigidbody2D.velocity = direction;  
		}  
	}  
}
```

- Event Controller에서 호출되는 Event Action을 구독 (Subscribe) 하여 메소드를 실행

```csharp
// Event Controller.cs

MoveEvent?.Invoke(value);  

// Player Action.cs

_movementEventController.MoveEvent += Move;
```

- Event 가 Invoke 되어 호출되면, 해당 이벤트를 지켜보며 구독하는 Move 메소드가 실행
- 이때, 매개변수를 전달안해도 되는 이유는 Event Action에서 값 자체를 같이 전달 해주기 때문에 따로 설정할 필요가 없음

```csharp
private void Move(Vector2 moveDirection)  
{  
	_moveDirection = moveDirection;  
}
```

- 추가적으로 물체가 이동할때는 Vector 값이 존재하지만 키 이동을 멈추면 값이 없지만 기존 값이 유지 되어 계속해서 운동하기 때문에 조건으로 Vector 정규화 중 `zero`를 사용하여 강제로 운동 값을 초기화 해주어야 함

```csharp
private void LateUpdate()  
{  
	MoveToward(_moveDirection != Vector2.zero ? _moveDirection : Vector2.zero);  
}
```


마치며
--

- Input System은 기존에 사용하던 코드보다 견결하고 지속적을 확인을 하는 것이 아닌 이벤트 드리븐 (Event Driven) 방식으로 최적화 및 효율성 그리고 크로스 플랫폼에서 사용 가능한 확장성을 가지게 되어 좀더 유용한 시스템 이 될 것이기 때문에 익숙해지며 응용방법을 좀더 찾아보야 할 것 입니다.



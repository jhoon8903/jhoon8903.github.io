---
title: Unity Input System
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/a8X67jG.jpg
comments_disable: true
meta_description: Unity Input System
---
# Input System
## Legacy Input System 구조

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

### Input Action 설정
#### 1. Action 생성
![](https://i.imgur.com/sZACsK1.jpg)

#### 2. Action Asset 실행
![](https://i.imgur.com/TFTM0mH.jpg)

#### 3.Control Schemes Add
![](https://i.imgur.com/p51WL9t.jpg)
#### 4. Actions 생성
![](https://i.imgur.com/IiRzR6i.jpg)
#### 5. Properties 설정
![](https://i.imgur.com/UJUVB8Q.jpg)
#### 6. 바인딩
![](https://i.imgur.com/kWjJYuh.jpg)


## Player Input Component
- Input Actions에서 정의한 액션이 발생했을 때 코드와 연결해 해당 로직을 실행시킬 수 있는 기능을 처리
![](https://i.imgur.com/1AeJCqm.jpg)
⭕️ 기본 컴포넌트

![](https://i.imgur.com/VEdGtNa.jpg)
🔴 미리 생성한 Actions를 연결하였을 때

### Behavior
- Input Actions 에셋에 정의한 액션이 발생했을 때 코드릐 함수를 어떻게 실핼시킬 것인지 결정
![](https://i.imgur.com/4wBQCww.jpg)

|Behavior Options|설명|
|:--|:--|
|Send Message|`Player Input 컴포넌트가 있는 게임오브젝트`에<br>SendMessage 함수를 이용하여 호출|
|Broadcast Message|Send Message와 동일하지만 BroadcastMessage함수 이용<br>하위에 있는 게임오브젝트의 함수도 호출|
|Invoke Unity Events|액션별로 이벤트 연결 인터페이스가 생성되고,<br>각각 호출하려는 함수를 연결해 사용|
|Invoke C Sharp Events|C# 스크립트에서 이벤트를 직접 연결해 사용|

- 위의 네가지 Behavior 속성은 입력작치의 연결, 해지, 변경사항에 대해 기본 이벤트를 호출
	- OnDeviceLost
	- OnDeviceRegained
	- OnControlsChanged

#### Send Message
- Input Actions에서 설정한 액션을 SendMessage를 통해 호출
##### 호출 함수 형식
```csharp
접근제한자 void On{Action}();
```
- Action명은 이미 Input Action에서 설정 (Move, Attack)
![](https://i.imgur.com/farCu91.jpg)
- On{Action}은  `OnMove`와 `OnAttack`이 됨

```csharp
public class PlayerCtrl : MonoBehaviour  
{  
  
	private Animator _anim;  
	private Transform _transform;  
	private Vector3 _moveDir;  
	  
	private void Start()  
	{  
		_anim = GetComponent<Animator>();  
		_transform = GetComponent<Transform>();  
	}  
	  
	private void Update()  
	{  
		if (_moveDir != Vector3.zero)  
		{  
			// 진행방향으로 회전  
			_transform.rotation = Quaternion.LookRotation(_moveDir);  
			// 회전한 후 전진 방향으로 이동  
			_transform.Translate(Vector3.forward * Time.deltaTime * 4.0f);  
		}  
	}  

	// 접근제한자 반환 On{액션명}();
	private void OnMove(InputValue value)  
	{  
		var dir = value.Get<Vector2>();  
		// 2차원 좌표를 3차원 좌표로 변환  
		_moveDir = new Vector3(dir.x, 0, dir.y);  
		// Warrior_Run 애니메이션 실행  
		_anim.SetFloat("Movement", dir.magnitude);  
		Debug.Log($"Move = ({dir.x}, {dir.y})");  
	}  
	
	// 접근제한자 반환 On{액션명}();
	private void OnAttack()  
	{  
		Debug.Log("ATTACK");  
		_anim.SetTrigger("Attack");  
	}  
}
```

![](https://i.imgur.com/LZSlx1V.gif)

#### Invoke Unity Events 옵션

![](https://i.imgur.com/UCRRksB.jpg)

##### 호출 함수 형식
```csharp
접근제한자 void On{Action}(InputAction.CallbackContext context)
{
	
}
```
- 해당  타입 선택시 넘어오는 Params 값은 `InputAction.CallbackContext` 타입
- 입력값은 `ReadValue<T>()` 함수를 이용해 전달 받음
- 이전 Action Move 값은 Vector2 타입으로 정의 하여서 받을 때도 Vector2로 받아야 함
```csharp
#region UNITY_EVENTS  
  
	public void OnMove(InputAction.CallbackContext ctx)  
	{  
		var dir = ctx.ReadValue<Vector2>();  
		// 2차원 좌표를 2차원 좌표로 변환  
		_moveDir = new Vector3(dir.x, 0, dir.y);  
		// Warrior_Run 애니메이션 실행  
		_anim.SetFloat("Movement", dir.magnitude);  
	}  
		  
	public void OnAttack(InputAction.CallbackContext ctx)  
	{  
		Debug.Log($"ctx.phase = {ctx.phase}");  
		  
		if (ctx.performed)  
		{  
			Debug.Log("ATTACK");  
			_anim.SetTrigger("Attack");  
		}  
	}  
#endregion
```

- 해당 옵션을 사용해 연결한 함수는 총 3번 호출 됨
![](https://i.imgur.com/ZL7Rm6U.gif)
- 시작, 실행, 취소의 Callback 함수를 각각 한 번씩 호출하여 어떤 상태로 호출 되었는지 확인은<br>CallbackContext.phase 속성을 통해 가능 (열거형)
```csharp
InputActionPhase.Started
InputActionPhase.Performed
InputActionPhase.Canceled
InputActionPhase.Disabled
InputActionPhase.Waiting
```

![](https://i.imgur.com/0NOSET1.jpg)


#### Invoke C Sharp Events
![](https://i.imgur.com/jpKE4Vc.jpg)
- Input Actions 에셋과 Player Input 컴포넌트 없이 모두 다 스크립트로 처리할 수 있음
```csharp
// Invoke C Sharp Events  
private PlayerInput _playerInput;  
private InputActionMap _mainActionMap;  
private InputAction _moveAction;  
private InputAction _attackAction;  
  
private void Start()  
{  
	_anim = GetComponent<Animator>();  
	_transform = GetComponent<Transform>();  
	  
	// Player Input 할당  
	_playerInput = GetComponent<PlayerInput>();  
	// ActionMap 추출  
	_mainActionMap = _playerInput.actions.FindActionMap("PlayerActions");  
	// Move, Action 추출  
	_moveAction = _mainActionMap.FindAction("Move");  
	_attackAction = _mainActionMap.FindAction("Attack");  
	  
	// Move 액션의 Performed 이벤트 연결  
	_moveAction.performed += ctx =>  
	{  
		var dir = ctx.ReadValue<Vector2>();  
		_moveDir = new Vector3(dir.x, 0, dir.y);  
		// Warrior_Run Animation 실행  
		_anim.SetFloat("Movement", dir.magnitude);  
	};  
	// Move 액션의 Canceled 이벤트 연결  
	_moveAction.canceled += ctx =>  
	{  
		_moveDir = Vector3.zero;  
		// Warrior_Run Animation Cancel  
		_anim.SetFloat("Movement", 0.0f);  
	};  
	  
	// Attack Action 연결  
	_attackAction.performed += ctx =>  
	{  
		Debug.Log($"Attack by C# events");  
		_anim.SetTrigger("Attack");  
	};  
}
```

![](https://i.imgur.com/L6d2dZy.gif)

#### Direct Binding
- PlayerInput 컴포넌트 없이 직접 InputAction을 생성 정의 하는 방법
```csharp
public class PlayerCtrlByEvent : MonoBehaviour  
{  
	private InputAction _moveAction;  
	private InputAction _attackAction;  
	private Animator _anim;  
	private Vector3 _moveDir;  
	private static readonly int Movement = Animator.StringToHash("Movement");  
	private static readonly int Attack = Animator.StringToHash("Attack");  
	  
	// Start is Called Before The First Frame Update  
	private void Start()  
	{  
		_anim = GetComponent<Animator>();  
		  
		// Configure Move Action's Composite Binding or Type  
		_moveAction = new InputAction("Move");  
		  
		// Define to Move Action's Composite Binding Information  
		_moveAction.AddCompositeBinding("2DVector")  
		.With("Up", "<Keyboard>/w")  
		.With("Down", "<Keyboard>/s")  
		.With("Left", "<Keyboard>/a")  
		.With("Right", "<Keyboard>/d");  
		  
		// Move 액션의 performed, canceled 이벤트 연결  
		_moveAction.performed += ctx =>  
		{  
			var dir = ctx.ReadValue<Vector2>();  
			_moveDir = new Vector3(dir.x, 0, dir.y);  
			// Warrior_Run 애니메이션 실행  
			_anim.SetFloat(Movement, dir.magnitude);  
		};  
		  
		_moveAction.canceled += ctx =>  
		{  
			_moveDir = Vector3.zero;  
			_anim.SetFloat(Movement, 0.0f);  
		};  
		  
		// Move 액션의 활성와  
		_moveAction.Enable();  
		// Attack 액션 생성  
		_attackAction = new InputAction("Attack", 
										InputActionType.Button, 
										"<Keyboard>/space");  
		  
		// Attack 액션의 performed 이벤트 연결  
		_attackAction.performed += ctx =>  
		{  
			_anim.SetTrigger(Attack);  
		};  
		  
		// Attack 액션의 활성화  
		_attackAction.Enable();  
	}  
	  
	private void Update()  
	{  
		if (_moveDir != Vector3.zero)  
		{  
			// 진행 방향으로 회전  
			transform.rotation = Quaternion.LookRotation(_moveDir);  
			// 회전한 후 전진 방향으로 이동  
			transform.Translate(Vector3.forward * Time.deltaTime * 4.0f);  
		}  
	}  
}
```

##### AddCompositeBinding (복합 바인딩)
- 복합 바인딩 타입 (Composite Type)은 문자열 "2DVector" 사용 (띄어쓰기 없음 ⛔️ )
- `.With`구문으로 필요한 만큼 바인딩 추가 가능
```csharp
AddCompositeBinding("Composite Type").With("Binding Name", "Binding Key");
```
- 지원 가능한 Composite Type
	- 1DAxis (or Axis)
	- 2DVector (or Dpad)
	- 3DVector
	- OneModifier
	- TwoModifiers

## Input Debug
- 입력 장치로부터 전달되는 값을 모니터링하는 기능
- 녹화, 저장, 로드 가능
![](https://i.imgur.com/YtlTnJa.jpg)
![](https://i.imgur.com/v35se0h.jpg)
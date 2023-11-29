---
layout: post
title: Avatar의 이름을 바꾸기 
subtitle: 
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
 - Event
banner:
  image: https://i.imgur.com/gGWrWj7.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

In Game Avatar Name Changed
--

- 인게임 내에서 아바타의 이름을 변경하는 기능을 구현

UI 구현
--

![](https://i.imgur.com/nDBNO8N.jpg)

![](https://i.imgur.com/DuCXYvt.jpg)

#### 어?! 버튼 또는 InputField가 안눌려요!

- Canvas 에서 Image UI를 만드시고 버튼 또는 UI 클릭이 눌리지 않는다면 해당 오브젝트의 부모컴포넌트에 `Graphic Raycaster` 컴포넌트를 추가 해주시기 바랍니다.

![](https://i.imgur.com/nJHtBCt.jpg)

- UI  - Image 의 경우 마우스 클릭 같은 Raycast가 무시되는 경우가 있습니다.
- 이때 해당 Raycast를 '인식'하기 위해 `Graphic Raycast`를 사용합니다.
- UI element 가 아닌 오브젝트인 경우 `Physics Raycast`를 사용합니다.


스크립트 작성
--

- 이름변경 패널을 활성화 시킬 버튼 이벤트 연결
- Input Field 값을 전달 받을 수 있는 OnEndEdit 메서드
- 이름을 변경할 때 2자 이상 10자 미만에만 버튼을 누를 수 있는 예외처리
- 이름변경 버튼을 눌렀을 때 이름변경에 데이터를 전달하는 기능
- 변경 버튼이 눌렸을 때 해당 패널을 닫을 수 있는 버튼

### ChangedName.cs

- 아바타의 이름을 바꾸고 GameManager의 EventHandler를 호출합니다.

```csharp
public class ChangedName : MonoBehaviour  
{  
  
	[SerializeField] private Button openBtn;  
	[SerializeField] private Button confirmBtn; 
	
	// input Field를 할당하기 위한 타입 TMP_InputField 
	[SerializeField] private TMP_InputField textMesh;  
	private GameManager _gameManager;  
	private string _changeNameText;  
	  
	/// <summary>  
	/// 이름 바꾸기 기능 초기화  
	/// </summary>  
	private void Awake()  
	{  
		confirmBtn.onClick.AddListener(ChangeName);  
		openBtn.onClick.AddListener(OpenPanel);  
		textMesh.onEndEdit.AddListener(OnInputFieldEndEdit);  
	}  
	  
	/// <summary>  
	/// GameManager를 의존성 주입 후 패널 비활성화  
	/// </summary>  
	private void Start()  
	{  
		_gameManager = ServiceLocator.GetService<GameManager>();  
		gameObject.SetActive(false);  
	}  
	  
	/// <summary>  
	/// 이름 바꾸기 패널 활성화, 캐릭터가 움직이는 것을 막기 위해  
	/// bool 값으로 상태 변경  
	/// </summary>  
	private void OpenPanel()  
	{  
		gameObject.SetActive(true);  
		_gameManager.InGameChangingName = true;  
	}  
	  
	/// <summary>  
	/// 이름 바꾸기에서 InputField 에서 들어온 이벤트를 호출하여 변수에 할당  
	/// </summary>  
	/// <param name="changeName"></param>  
	private void OnInputFieldEndEdit(string changeName)  
	{  
		_changeNameText = changeName;  
	}  
	  
	/// <summary>  
	/// 닫기 버튼을 누르면 GameManger에 EventHandler 호출  
	/// 창이 닫히면 캐릭터를 다시 움직일 수 있도록 bool 값 변경  
	/// </summary>  
	private void ChangeName()  
	{  
		// EventHandler 호출
		_gameManager.ChangeName(this,_changeNameText); 

	   // 패널 닫기
		gameObject.SetActive(false);  

		// 캐릭터 움직임 원상복구
		_gameManager.InGameChangingName = false;  
	}  
}
```

### EventHandler 를 이용하기 위한 EventArgs 클래스 작성

- EventHandler의 매개변수인 `EventArgs` 를 이용하기 위해서는 별도의 클래스 필요
- 해당 클래스는 `EventArgs`를 `상속`받아야 사용이 가능합니다.

```csharp
public class NameChangedEventArgs : EventArgs  
{  
	public string NewName { get; set; }  
	public string OldName { get; set; }  

	// 생성자를 만들어 줍니다.
	public NameChangedEventArgs(string oldName, string newName)  
	{  
		OldName = oldName;  
		NewName = newName;  
	}  
}
```

### GameManager.cs

- 변경 된 이벤트를 적용하기 위한 Eventhandler를 생성합니다.

```csharp
/// <summary>  
/// 이름을 바꾸는 이벤트 핸들러  
/// </summary>  
public event EventHandler<NameChangedEventArgs> OnChangedName;  

private string _currentName;  

public void ChangeName(object sender, string newName)  
{  
	if (sender is LoginPanel.LoginPanel)  
	{  
		_currentName = null;  
	}  
	var args = new NameChangedEventArgs(_currentName, newName);  
	_currentName = newName;  
	OnChangedName?.Invoke(sender, args);  
}
```

- 이름을 변경하는 이벤트의 경우 여러가지 케릭터 또는 Side Panel에 인스터스화 되어있는 Users 의 경우 특정 이름의 인스턴스만 변경하기 위해서 현재 이름과 새로운 이름을 모두 받아서 두 string 값을 모두 전달 해줍니다.
- 이름을 받은 객체는 현재이름과 새로운 이름을 비교하여 조건에 부합하는 객체의 이름만 변경합니다.
- 하지만 처음 로그인 하는 경우 이름이 없기 때문에 EventHandler의 sender 변수를 확인하여, sender 가 LoginPanel 객체일 경우는 현재 이름 (_currentName)을 null로 전달 합니다.
- 이후 새로운 이름은 현재 이름으로 재할당 해줍니다.

### CharacterAction.cs

- 변경 된 이름을 적용하기 위해 캐릭터 오브젝트는 이벤트를 구독합니다.
- 이름 변경 이벤트를 송신 받으면 이름변경 메서드를 실행합니다.

```csharp
private void Start()  
{  
	... 기존 코드

	// 이름변경 이벤트 감지 시 UpdateName 메서드 실행
	_gameManager.OnChangedName += UpdateName;  
	
	CharacterInitialize();  
}

/// <summary>  
/// 현재 이름과 비교하여 OldName과 현재 이름이 같다면, 새로운 이름으로 교체 합니다.  
/// </summary>  
/// <param name="sender"> 발신자 정보 </param>  
/// <param name="e"> e.OldName, e.NewName </param>  
private void UpdateName(object sender, NameChangedEventArgs e)  
{  
	if (e.OldName == characterName.text)  
	{  
		characterName.text = e.NewName;  
	}  
}
```

### Users.cs

- 현재 SidePanel에 인스턴스화 되어 있는 Users 객체의 이름 또한 업데이트 해줍니다.
- 여러 인스턴스 중 `현재 이름 (_currentName)`과 `OldName`이 일치하는 오브젝트만 교체 합니다.

```csharp
private string _currentName; // 현재 이름


/// 이벤트로 들어온 Name과 Sprite 를 저장 해둡니다.  
private void Start()  
{  
	_gameManager = ServiceLocator.GetService<GameManager>();
	// Name 변경이벤트 감지  
	_gameManager.OnChangedName += UpdateName; 
	// Sprite 변경이벤트 감지 
	_gameManager.OnCharacterThumbnailChanged += UpdateSprite;  
}

/// <summary>  
/// 이벤트를 호출 받으면 해당 이름을 비교하여 _currentName과 e.OldName이 일치하는 객체만 변경합니다.  
/// </summary>  
/// <param name="sender"> 발신자 </param>  
/// <param name="e"> e.OldName, e.NewName </param>  
private void UpdateName(object sender, NameChangedEventArgs e)  
{  
	if (e.OldName != _currentName) return;  
	userName.text = e.NewName;  
	_currentName = userName.text;  
}

/// <summary>  
/// 스프라이트 변경 메서드가 들어오면  
/// </summary>  
/// <param name="sprite"></param>  
private void UpdateSprite(Sprite sprite)  
{  
	userAvatar.sprite = sprite;  
}
```



![](https://i.imgur.com/gGWrWj7.gif)


문제점 
--

기존 코드에서 추가적인 기능들을 개발하면서 문제점을 발견했습니다.
싱글 플레이에서는 플레이어 1명 이어서 문제가 없지만, 멀티 플레이 상황이라고 하면 여러 유저들 중 현재 유저를 추적하여 해당 유저의 정보만 변경할 수 있는 로직이 없기 때문입니다.

다음 프로젝트에서 해당 기능을 구현 한다고 하면 처음부터 플레이어를 지속적으로 추적할 수 있는 불변 (Prime) 값을 설정하여 각 유저를 추적하고 이를 확인하여 플레이어를 특정 하고 해당 플레이어의 정보만 변경 할 수 있도록 해야 합니다.

해당 스크립트를 따라오면 기본적인 과제에 대한 문제는 없지만 지속적으로 확장은 매우 불편한 코드가 되었습니다.

다음 부터는 설계부터 제대로 생각하여 문제 없고 혹시나 발생할 수 있는 확장성을 생각하여 서비스 코드를 구현 해야 합니다.
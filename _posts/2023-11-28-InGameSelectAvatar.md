---
layout: post
title: Avatar 교체 (feat. EventHandler, Action Event)
subtitle: Eventhandler와 Action도 있어요
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
 - Event
banner:
  image: https://i.imgur.com/WZcgdpX.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

InGame Avatar Change
--

- 캐릭터 선택 화면이 아닌 인게임 상에서 Avatar를 바꾸는 기능에 대한 구현

UI 구성
--
### Avatar 교체 버튼

![](https://i.imgur.com/pKFiOnN.jpg)

![](https://i.imgur.com/ZgR4wcH.jpg)

### Avatar 선택 Panel

- 해당 케릭터 들은 인스턴화 하여 사용할 예정이므로 별도의 프리팹으로 구성합니다.

![](https://i.imgur.com/ozbiLwI.jpg)

![](https://i.imgur.com/X7TxyTH.jpg)

#### 문제점 캐릭터의 이름이 Panel 위에 나오는 문제

- 케릭터의 Name Tag 가 Avatar 선택 패널보다 위에 올라와서 발생하는 문제
- 선택 패널에 별도의 `Canvas` 컴포넌트를 추가하여 `Override Sorting`체크
- 체크 후에 Sort Order를 Name Tag 보다 위로 설정하면 됩니다.


![](https://i.imgur.com/29jft4y.jpg)

![](https://i.imgur.com/31eYRyB.jpg)

- Avatar 프리팹의 경우 컴포넌트로 `Button`을 넣어 onClick 이벤트기능을 사용 가능하도록 합니다.

![](https://i.imgur.com/ARXSslq.jpg)


스크립트 작성
--
- Avatar 패널 실행버튼
- Avatar 패널 활성화 / 비활성화
- Avatar 프리팹 인스턴스
- Avatar 프리팹 선택 
- 선택한 Avatar 정보를 Player 캐릭터에 전달
- 캐릭터를 전달 된 데이터로 업데이트

### Avatar Data 옮기기
- 기존 Login Panel 에 있던 Characters List 를 GameManager로 옮깁니다.
- 이를 통해서 다른 곳에서도 GameManager에서 Character List를 가져옵니다.

**기존 CharacterCard.cs**

```csharp
public class CharacterCard : MonoBehaviour
{
	private GameManager _gameManager;

	#region Character InformationList
	/// <summary>
	///  Character Class 로 케릭터의 타입(enum), 이미지 (Sprite), 인게임에서 사용할 컨트롤러(AnimatorController) 설정,
	///  인스펙터 상에서 characters 리스트 형태로 할당하여 정보를 저장  
	/// </summary>
	[Serializable] public class Character
	{
		public enum CharacterTypes { FROG, PINKYMAN, ZEP }   // 케릭터의 유니크 타입 목록
		public CharacterTypes characterType;    // 케릭터의 유니크 타입값
		public Sprite characterImage;   // 케릭터의 기본 Sprite 
		public AnimatorController animator; // 인게임에서 사용할 케릭터 컨트롤러
	}

	[SerializeField] private List<Character> characters;    // Character Class를 리스트 형태로 관리
	#endregion

	#region Default Character Initilaze
	/// <summary>
	///  기본 InputField 화면의 기본 케릭터 설정
	/// </summary>
	private void Start()
	{
		_gameManager = ServiceLocator.GetService<GameManager>();
		Character defaultCharacter = characters[0];
	}

	private void InstantiateSelectCard()
	{
		foreach (var character in characters)
		{
			SelectCard cardInstance = Instantiate(selectCardPrefab, selectPanelTransform);
			cardInstance.CharacterType = character.characterType;
			cardInstance.CharacterSprite = character.characterImage;
			cardInstance.Controller = character.animator;
		}
	}
	#endregion
}
```

**변경 된 CharacterCard.cs**

```csharp
public class CharacterCard : MonoBehaviour  
{  
	private GameManager _gameManager;  
	  
	#region Character Select Variables  
	[SerializeField] private Transform selectPanelTransform; // 케릭터 카드를 랜더링할 부모 오브젝트의 위치  
	[SerializeField] private SelectCard selectCardPrefab; // 인스턴스화 할 케릭터 프리팹  
	
	private List<GameManager.Character> _characterList = new List<GameManager.Character>();  
	#endregion  
	  
	#region Default Character Initilaze  
	/// <summary>  
	/// 기본 InputField 화면의 기본 케릭터 설정  
	/// </summary>  
	private void Start()  
	{  
		_gameManager = ServiceLocator.GetService<GameManager>();  
		_characterList = _gameManager.characters;  
		GameManager.Character defaultCharacter = _characterList[0]; 
	}  
	  
	private void InstantiateSelectCard()  
	{  
		foreach (var character in _characterList )  
		{  
			SelectCard cardInstance = Instantiate(selectCardPrefab, selectPanelTransform);  
			cardInstance.CharacterType = character.characterType;  
			cardInstance.CharacterSprite = character.characterImage;  
			cardInstance.Controller = character.animator;  
		}  
	}  
	#endregion  
}
```

- 캐릭터 리스트를 GameManger에 위치 시키고 Service Locator 패턴을 이용하여 의존성을 주입합니다.
- GameManger의 Inspector에서 다시 캐릭터 세팅을 해줍니다.

![](https://i.imgur.com/p6RZrlq.jpg)


### GameMangaer 이벤트 만들기

- Avatar 인스턴스를 생성시에 Avatar를 선택하면 선택패널이 닫히면서 Avatar 변경을 적용할 EventHandler 생성

#### GameManager.cs

```csharp
public event EventHandler OnClosePanelHandler;  
public void ClosePanel()  
{  
	OnClosePanelHandler?.Invoke(this, EventArgs.Empty);  
}
```

#### event Action<> 과 EventHandler 의 차이에 대해서

**Delegate**

> Action<`T`> : T 타입의 단일 매개변수를 취하고 void 를 반환하는 메서드
> 
> EventHandler<`T`>: 2가지의 매개변수 (`object sender, EventArgs args`)의 매개변수를 취하는 사전 설계된 메서드, 여기서 <`T`>는 `EventArgs`에서 파생 되어야 함

**Sender**

> Action<`T`>의 경우에는 누가 이벤트를 발생 시켰는지에 대한 정보가 없어서, 발신자 참조가 필요없는 간단한 이벤트를 처리할 때 사용
>
> EventHandler<`T`>의 경우에는 발신자가 누구인지를 아는게 중요한 데이터를 전달 할 때 사용됩니다. 만약 같은 이벤트를 공유하면서 발신자에 따라 다르게 로직을 처리 해야하는 상황에 사용됩니다.

**정리**

> 매개변수
>
> Action 의 경우 `T`하나의 매개변수만을 취하지만,     
> EventHandler의 경우 `object sender`와 `EventArgs e` 두가지의 매개변수를 취함

> 사용처
> 
> 복잡한 시나리오에 사용시에는 `EventHandler` 간단한 사용은 `Action`

> 유연성
> 
> Action<`T`>는 어떠한 타입도 될 수 있는 유연성을 지니고 있으며,   
> EventHandler<`T`>의 경우 `EventArgs`에서 파생된 타입만이 가능하다는 제안이 있습니다.


**사용방법**

- Action<`T`>
- Input Controller에서 키 입력 이벤트를 발생 시킬 때 이때 매개변수는 (`Vector2`)

```csharp
public event Action<Vector2> MoveEvent;

public void CallMoveEvent(Vector2 value)
{
	MoveEvent?.invoke(value);
}

// 다른 클래스에서 이벤트를 수신 받을 때

private void SomeoneMethod()
{
	_movementEventController.MoveEvent += Move;
}

private void Move(Vector2 value)
{
	_moveDirection = value;
}
```


- EventHandler<`T`>
- 패널을 닫는다는 메세지를 특정 클래스에서 다른 인스턴스 또는 클래스를 호출하여 발신자 마다 서로 다른 패널을 닫을 때

```csharp
public event EventHandler OnClosePanelHandler;

public void ClosePanel(object sender)
{
	OnClosePanelHandler?.Invoke(sender, EventArgs.Empty);
}

// 다른 클래스에서 이벤트를 수신 받을 때

private void ClosePanel(object sender, EventArgs e)  
{  
	switch (sender)  
	{  
		case AvatarInstance:  
			avatarSelectPanel.gameObject.SetActive(false);  
			break;  
	}  
}
```

#### AvatarInstance.cs

- 아바타 오브젝트가 되어 화면에 표시 될 Prefab 오브젝트의 스크립트

```csharp
public class AvatarInstance : MonoBehaviour  
{  
	[SerializeField] private Image avatar;  
	[SerializeField] private TextMeshProUGUI avatarName;  
	private Button _selectBtn;  
	private GameManager _gameManager;  
	private AnimatorController _controller;  
	  
	/// <summary>  
	/// GameManager Service 등록  
	/// </summary>  
	public void Start()  
	{  
		_gameManager = ServiceLocator.GetService<GameManager>();  
		_selectBtn = GetComponent<Button>();  
		_selectBtn.onClick.AddListener(SelectedAvatar);  
	}  
	  
	/// <summary>  
	/// 인스턴스화 시에 할당할 세팅 메서드  
	/// </summary>  
	/// <param name="type">케릭터의 이름</param>  
	/// <param name="avatarSprite">케릭터의 이미지</param>  
	/// <param name="controller">선택시에 캐릭터에 넘겨 줄 애니메이터 컨트롤러</param>  
	public void SetAvatar (CharacterTypes type, Sprite avatarSprite, AnimatorController controller)  
	{  
		avatar.sprite = avatarSprite;  
		avatarName.text = type.ToString();  
		_controller = controller;  
	}  
	  
	/// <summary>  
	/// 선탣된 아바타의 컨르롤러 정보를 GameManager에 전달  
	/// </summary>  
	private void SelectedAvatar()  
	{  
		_gameManager.CharacterController = _controller;  
		_gameManager.ClosePanel(this);  
	}  
}
```

#### SelectedAvatar.cs

- 인게임 에서 아바타 선택을 위해서 실행되는 아바타 선택화면 클래스

```csharp
public class SelectedAvatar : MonoBehaviour  
{  
	[SerializeField] private Image avatarSelectPanel;  
	[SerializeField] private AvatarInstance avatarInstance;  
	private GameManager _gameManager;  
	private Button _openBtn;  
	private List<GameManager.Character> _avatarList = new List<GameManager.Character>();  
	private AvatarInstance _avatar;  
  
	/// <summary>  
	/// 스크립트 초기화 GameManager 서비스를 검색하고, 버튼 이벤트 활성화  
	/// </summary>  
	private void Start()  
	{  
		_gameManager = ServiceLocator.GetService<GameManager>();  
		_gameManager.OnClosePanelHandler += ClosePanel;  
		_avatarList = _gameManager.characters;  
		_openBtn = GetComponent<Button>();  
		_openBtn.onClick.AddListener(OpenAvatarSelectPanel);  
	}  
  
	/// <summary>  
	/// 선택 화면 패널 활성화  
	/// </summary>  
	private void OpenAvatarSelectPanel()  
	{  
		avatarSelectPanel.gameObject.SetActive(true);  
		InstantiateAvatar();  
	}  
  
	/// <summary>  
	/// 패널이 닫히는 이벤트 수신 메서드  
	/// sender가 AvatarInstance 일때만 패널이 닫힙니다.  
	/// </summary>  
	/// <param name="sender">이벤트 메세지의 발신자</param>  
	/// <param name="e">EventArgs 매개변수 여기서는 EventArgs.Empty</param>  
	private void ClosePanel(object sender, EventArgs e)  
	{  
		switch (sender)  
		{  
			case AvatarInstance:  
				avatarSelectPanel.gameObject.SetActive(false);  
				break;  
		}  
	}  
  
	/// <summary>  
	/// 패널이 열릴때 AvtarInstance를 인스턴스화  
	/// 중복 생성을 방지 하기 위해서 Null 일때만 생성하고 그 외에는 return 처리  
	/// </summary>  
	private void InstantiateAvatar()  
	{  
		if (_avatar != null) return;  
		foreach (var character in _avatarList)  
		{  
			_avatar = Instantiate(avatarInstance, avatarSelectPanel.transform);  
			_avatar.SetAvatar(character.characterType, character.characterImage, character.animator);  
		}  
	}  
}
```


이 기능들에서 중요한 것은 오브젝트를 인스턴스화 하는 것 보다 
EventHandler와 Action의 차이를 알고 필요에 따라 사용하는 방법을 익히는 것 입니다.

![](https://i.imgur.com/WZcgdpX.gif)

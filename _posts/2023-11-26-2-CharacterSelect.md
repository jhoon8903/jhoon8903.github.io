---
layout: post
title: 케릭터 선택 기능
subtitle: 프리팹 인스턴스를 이용한 케릭터 선택 기능
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
 - Instance
 - Prefabs
banner:
  image:
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)


![](https://i.imgur.com/pitJw5n.gif)

- 케릭터의 선택 및 게임상의 케릭터를 재할당하여 상태를 변하게 하는 기능의 구현
### UGUI 구성

#### 케릭터 선택 버튼 UI

- 먼저 케릭터를 선택후 선택 된 케릭터를 랜더링할 오브젝트를 생성합니다.
- 그리고 나서 해당 오브젝트에 Button 컴포넌트를 추가하여 버튼을 눌렀을 때 케릭터 선택 창이 나올 수 있도록 합니다.
- 그리고 버튼 컴포넌트 위에 케릭터를 표시할 이미지 컴포넌트를 하나 추가 합니다.

![](https://i.imgur.com/UdADz52.jpg)

- 버튼 UI의 Sprite를 선택하고 이미지는 별도로 랜더링할 예정이므로 위치만 설정합니다.

![](https://i.imgur.com/ILrEGgt.jpg)

- 해당 이미지에 Sprite만 너으면 아래 그림처럼 표시가 나오게 됩니다.

![](https://i.imgur.com/j1n5hfi.jpg)

#### 케릭터 리스트 패널

- 케릭터 선택 버튼을 눌렀을 때 활성화 될 선택패널을 생성합니다.

![](https://i.imgur.com/YNRFafy.jpg)

![](https://i.imgur.com/nhWrtfy.jpg)

- 해당 패널에는 케릭터의 숫자 만큼 케릭터가 정령되어야 하므로 `Grid` 또는 `Horizontal` Layout Group 컴포넌트를 추가 합니다 (여기서는 `Grid Layout Group`을 사용)
- 해당 레이아웃에서 표시될 자녀 오브젝트의 사이즈를 맞추고 Padding 값을 조절하여 정렬 합니다.

![](https://i.imgur.com/aYdq6PT.jpg)

- Layout Group을 사용하면 해당 설정에 맞게 오브젝트가 추가 될 때마다 알아서 정렬을 해줍니다.

![](https://i.imgur.com/etDl9Cx.jpg)

![](https://i.imgur.com/Yr2UqFa.jpg)

### 선택 실행 버튼 Script (LoginPanel)

![](https://i.imgur.com/HDHIj2z.jpg)

- 이제 해당 오브젝트를 실행 시킬 Script를 작성합니다.

```csharp
#region Selected Button Variables  
[SerializeField] private Button characterSelectBtn; // 케릭터 선택 패널 실행 버튼 오브젝트  
[SerializeField] private Image characterSelectPanel; // 케릭터 선택 패널 오브젝트  
[SerializeField] private Image characterThumbnail; // 케릭터 선택 버튼 위에 보여줄 케릭터 이미지  
#endregion

#region Initialize Default Value  
/// <summary>  
/// LoginPanel 초기화 메서드  
/// 인풋 필드 및 로그인 화면 케릭터 기본 값 세팅  
/// </summary>  
private void Start()  
{  
	InitializedInPutField(); // 인풋 필드 초기화  
	InitializedDefaultCharacter(); // 케릭터 세팅 초기화 (FROG)  
  
	// 케릭터가 선택 되면 해당 케릭터 Sprite를 바꿔주는 이벤트  
	GameManager.Instance.OnCharacterThumbnailChanged += CharacterSelectPanelClose;  
}  
#endregion

#region Character Select Method Group  
private void InitializedDefaultCharacter()  
{  
	// 케릭터 선택 버튼을 클릭하면 케릭터 선택 패널이 열리도록 이벤트 구독  
	characterSelectBtn.onClick.AddListener(CharacterSelectPanelOpen);  
	// 케릭터 기본 이미지 설정  
	characterThumbnail.sprite = GameManager.Instance.CharacterThumbnail;  
}  
  
/// <summary>  
/// 케릭터 선택 패널 오픈 메서드  
/// </summary>  
private void CharacterSelectPanelOpen()  
{  
	// 케릭터 선택 패널 오브젝트 활성화  
	characterSelectPanel.gameObject.SetActive(true);  
}  
  
/// <summary>  
/// 선택된 케릭터 Sprite를 받아 선택된 케릭터를 랜더링 후 케릭터 선택 패널 비활성화  
/// </summary>  
/// <param name="sprite">선택한 케릭터 Sprite</param>  
private void CharacterSelectPanelClose(Sprite sprite)  
{  
	characterThumbnail.sprite = sprite; // 케릭터 이미지 교체  
	characterSelectPanel.gameObject.SetActive(false); // 케릭터 선택화면 비활성화  
}  
#endregion
```

- 버튼 타입의 경우 onClick AddListner 이벤트를 사용하여 클릭시 작동할 이벤트를 구성합니다.
- 추가적으로 GameManager에서 Thumbnail이 바뀔 때 마다 이벤트로 Login Panel의 Thumbnail 을 바꿔 줍니다.

### 케릭터 선택 Script (CharacterCard)

![](https://i.imgur.com/wEPHwS5.jpg)

![](https://i.imgur.com/EoSU5Bu.jpg)

- 케릭터선택 패널이 실행되면 케릭터의 수만큼 케릭터를 선택 패널에 랜더링할 기능을 구현
- 필요한 케릭터의 정보
	- 케릭터의 타입 (선택된 케릭터 구분용)
	- 케릭터의 선택 후 보여질 기본 이미지
	- 인게임에서 적용될 Animator Controller
- 기본적으로 아무것도 선택되지 않은 기본값을 설정할 수 있어야 합니다. 그렇지 않으면 케릭터 창이 빈 값으로 나오기 때문입니다.
- 케릭터 정보를 받아서 이것을 인스턴스화 하여 랜더링한 Instantiate 기능

#### 케릭터 정보 세팅

- 직렬화 된 케릭터 정보를 클래스로 만들어 해당 객체를 리스트로 관리

```csharp
#region Character InformationList  
/// <summary>  
/// Character Class 로 케릭터의 타입(enum), 이미지 (Sprite), 인게임에서 사용할 컨트롤러(AnimatorController) 설정,  
/// 인스펙터 상에서 characters 리스트 형태로 할당하여 정보를 저장  
/// </summary>  
[Serializable] public class Character  
{  
	public enum CharacterTypes { FROG, PINKYMAN } // 케릭터의 유니크 타입 목록  
	public CharacterTypes characterType; // 케릭터의 유니크 타입값  
	public Sprite characterImage; // 케릭터의 기본 Sprite  
	public AnimatorController animator; // 인게임에서 사용할 케릭터 컨트롤러  
}  
  
[SerializeField] private List<Character> characters; // Character Class를 리스트 형태로 관리  
#endregion
```

#### 인스턴스화 할 케릭터 프리팹 

- 인스턴스화 할 프리팹 및 인스턴스화 될 부모 오브젝트의 Transform 변수

```csharp
#region Character Select Variables  
[SerializeField] private Transform selectPanelTransform; // 케릭터 카드를 랜더링할 부모 오브젝트의 위치  
[SerializeField] private SelectCard selectCardPrefab; // 인스턴스화 할 케릭터 프리팹  
#endregion
```

#### 초기화 

- 케릭터의 기본 값을 세팅
- 케릭터 선택창에 랜더링 프리팹 인스턴스화

```csharp
#region Default Character Initilaze  
/// <summary>  
/// 기본 InputField 화면의 기본 케릭터 설정  
/// </summary>  
private void Start()  
{  
	Character defaultCharacter = characters[0];  
	GameManager.Instance.CharacterController = defaultCharacter.animator; // 기본 케릭터 Controller GameManager로 할당  
	GameManager.Instance.CharacterType = defaultCharacter.characterType; // 기본 케릭터 타입 GameManager로 할당  
	GameManager.Instance.CharacterThumbnail = defaultCharacter.characterImage; // 기본 케릭터 이미지 GameManager 할당  
	InstantiateSelectCard(); // 선택화면의 케릭터 오브젝트를 인스턴스화  
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
```

### GameManager

![](https://i.imgur.com/1l2jnPi.jpg)

- GameManger는 Singleton으로 인스턴스를 만들고 전역으로 접근 가능하도록 합니다.
- 각 변수는 케릭터의 정보를 담으며, 변수가 변경 되면 해당 이벤트를 발생시켜 Callback으로 해당 데이터를 넘겨 줍니다.

#### Singleton

- GameManager는 오직 하나만 존재하므로 Singleton으로 정적으로 접근 가능하도록 합니다.

```csharp
#region Singleton to GameManager  
public static GameManager Instance { get; private set; }  
  
/// <summary>  
/// GameManager 싱글턴 인스턴스 화  
/// </summary>  
private void Awake()  
{  
	if (Instance == null)  
	{  
		Instance = this;  
	}  
	else  
	{  
		Destroy(gameObject);  
	}  
	DontDestroyOnLoad(gameObject);  
}  
#endregion
```

#### 케릭터 정보 및 프로퍼티 이벤트 Invoke

- 이벤트의 경우 프로퍼티를 사용하여 설정 시에 자동적으로 해당 이벤트를 실행할 수 있도록 Set을 합니다.

```csharp
#region Character Information Variable  
private Sprite _characterThumbnail; // 이미지 변수  
public event Action<Sprite> OnCharacterThumbnailChanged; // 이미지 변경 이벤트  
private AnimatorController _characterController; // 컨트롤러  
public event Action<AnimatorController> OnCharacterControllerChanged; // 컨트롤러 변경 이벤트  
public CharacterTypes CharacterType { get; set; } // 케릭터 타입  
#endregion  
  
#region Changed Character Information Event Method  
/// <summary>  
/// Thumbnail 변경시 이벤트 변경  
/// </summary>  
public Sprite CharacterThumbnail // 케릭터 선택 패널에서 보여 줄 케릭터 스프라이트  
{  
	get => _characterThumbnail;  
	set  
	{  
		_characterThumbnail = value;  
		OnCharacterThumbnailChanged?.Invoke(_characterThumbnail);  
	}  
}  
  
/// <summary>  
/// Controller 변경 시 이벤트 변경  
/// </summary>  
public AnimatorController CharacterController  
{  
	get => _characterController;  
	set  
	{  
		_characterController = value;  
		OnCharacterControllerChanged?.Invoke(_characterController);
	}  
}  
#endregion
```


### Select Card 

- 선택창에 인스턴스화 되어 Group Layout 에 생성되는 카드 인스턴스 입니다.
- 해당 스크립트는 SelectCard 프리팹에 컴포넌트 할당합니다.
- 케릭터의 버튼을 선택하면 해당 인스턴스에 할당 된 정보를 GameManager 에 할당하여 변경 합니다.

![](https://i.imgur.com/XQaihKY.jpg)

![](https://i.imgur.com/cTZzDJh.jpg)

```csharp
#region Character Card Variables  
// 해당 케릭터가 인스턴화 될 때 설정되는 케릭터 정보  
public CharacterTypes CharacterType { get; set; }  
public Sprite CharacterSprite { get; set; }  
public AnimatorController Controller { get; set; }  
#endregion  
  
#region Object Variables  
private Button _selectBtn;  
private Image _characterImage;  
#endregion  
  
#region Initliaze Component  
/// <summary>  
/// 각 컴포넌트 및 이벤트 초기화  
/// </summary>  
private void Awake()  
{  
	_selectBtn = GetComponent<Button>(); // 버튼 컴포넌트 할당  
	_selectBtn.onClick.AddListener(Selected); // 버튼 클릭 이벤트 준비 (Callback => Selected)  
	_characterImage = GetComponent<Image>(); // 케릭터 컴포넌트 할당  
}  
  
/// <summary>  
/// 기본 인스턴스의 케릭터 이미지 세팅  
/// </summary>  
private void Start()  
{  
	_characterImage.sprite = CharacterSprite;  
}  
#endregion  
  
#region Callback Event  
/// <summary>  
/// 버튼 클릭 이벤트 발생시 Callback으로 실행되는 메서드  
/// GameManager에 해당 케릭터 정보를 전달 합니다.  
/// </summary>  
private void Selected()  
{  
	GameManager.Instance.CharacterType = CharacterType;  
	GameManager.Instance.CharacterController = Controller;  
	GameManager.Instance.CharacterThumbnail = CharacterSprite;  
}  
#endregion
```


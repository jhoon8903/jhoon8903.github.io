---
layout: post
title: 접속 상태자 확인
subtitle: 지금 접속해있는 사람은 누구?
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
 - UGUI
banner:
  image: https://i.imgur.com/oUGaUNm.gif
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

접속자 UGUI 제작
--

- 접속자를 표시하는 리스트 표시하는 기능에 대한 구현
### DEMO

![](https://file.notion.so/f/f/83c75a39-3aba-4ba4-a792-7aefe4b07895/73614b35-8386-4d5a-9a66-3a8b4c03263c/CleanShot_2023-09-01_at_04.32.22.gif?id=eaa1693b-f550-4dc3-bc06-5ba4e4a9c71d&table=block&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&expirationTimestamp=1701057600000&signature=9T1a3W-qIHAtCuyY9Dn0wyubm5UwnJr3E8PEjdkim5o)

- UI 구성 후 접속자 리스트는 GameManager에서 접속자 리스트를 받은 후 
- 해당 리스트 만큼 인스턴스로 출력하는 기능으로 구현할 예정
- 닫기 버튼의 경우 자연스럽게 사라질 수 있도록 애니메이션으로 구현할 예정

### 기본 UGUI 구성
#### 레이어
- InGameCanvas -> InGame용 Canvas 배경 (UI - Canvas)
	- UnderBar -> 하단 UI (UI - Image)
		- ListOpenBtn -> SideBar를 여는 버튼 아이콘 (UI - Image, Button)
	- SideBar -> 사이드 바 UI (UI - Image)
		- TitleText -> 상단 타이틀 (UI - Text Mesh Pro)
		- CloseBtn -> 닫기 버튼 (UI - Image)
		- PeopleViewArea -> 접속자 인스턴스를 생성할 아웃 Area (UI - Image)
			- ScrollView -> 인스턴스화된 리스트가 길어지면 스크롤할 아웃라인 (UI - Scroll View)
				- Viewport -> 인스턴스가 보이는 View (Scroll View 자동생성)
					- Content -> 인스턴스가 부모 위치(Scroll View 자동생성)
						- 유저 정보 인스턴스 -> Prefab 인스턴스
							- Avatar -> 유저 이미지
							- Name -> 유저 이름
					- Scrollbar Vertical -> 스크롤 바 (Scroll View 자동생성)
						- Sliding Area -> 슬라이딩 바 (Scroll View 자동생성)
							- Handle -> 슬라이딩 바 핸들 (Scroll View 자동생성)

![](https://i.imgur.com/tuOmkrc.jpg)


### UI Script 
#### UnderBar UI

- 하단 UI에서 SideBar를 열 수 있는 버튼을 `onclick.AddListner(OpenSideBar)` 등록합니다.
- Side Bar의 경우 처음에는 보이면 안되기 때문에 비활성화 합니다.

```csharp
#region UnderBar UI Variables  
// Open Button Object
[SerializeField] private Button openUserBtn;  
#endregion

#region SideBar UI Variables  
// Side Bar Object
[SerializeField] private Image sidebar;
#endregion

#region Initilaze UI Instance  
private void Awake()  
{  
	// Open SideBar Event
	openUserBtn.onClick.AddListener(OpenSideBar); 
	// 사이드바 비활성화
	sidebar.gameObject.SetActive(false);
}  
#endregion

private void OpenSideBar()  
{  
	// 사이드바 활성화
	sidebar.gameObject.SetActive(true);  
}
```

![](https://i.imgur.com/dyC3BcA.gif)

#### SideBar UI

- SideBar의 경우 닫기버튼, 유저 인스턴스화 기능을 수행합니다.

**변수** & **초기화**

```csharp
#region SideBar UI Variables  
[SerializeField] private Image sidebar; // 사이드바 오브젝트  
[SerializeField] private Button closeBtn; // 닫기 버튼 오브젝트  
[SerializeField] private Users userPrefab; // 접속 유저마다 1개씩 생성 되는 Users Prefab 오브젝트  
[SerializeField] private Transform contentTransform; // 인스턴스가 생성 될 부모 오브젝트 위치  
#endregion

#region Initilaze UI Instance  
/// <summary>  
/// 인게임 UI Initialize  
/// 버튼 이벤트 설정 및 사이드바 패널 초기상태를 설정 (비활성화)  
/// </summary>  
private void Awake()  
{  
	openUserBtn.onClick.AddListener(OpenSideBar); // 사이드바 onClick Open Event  
	closeBtn.onClick.AddListener(CloseSideBar); // 사이드바 onClick Close Event  
	sidebar.gameObject.SetActive(false); // 사이드바 오브젝트 초기 값 (비활성화)  
}  
#endregion
```


**인스턴스 오브젝트 및 사용자 정보 저장**

- SideBar가 열릴때 GameManager에서 접속자 이름과 케릭터 정보를 받고
- 이를 Users 프리팹 오브젝트에 할당하여 인원 수 만큼 인스턴스화 합니다.
- Users 스크립트를 프리팹에 추가하고, GameManager에 입장때 마다 유저 정보를 저장 합니다.

**Users.cs**

- Users Class의 경우 Avatar, Name 을 할당 받습니다.
- SetInformation 이 호출될 때 이를 세팅합니다.

```csharp
public class Users : MonoBehaviour  
{  
	[SerializeField] private Image userAvatar; // Image 컴포넌트  
	[SerializeField] private TextMeshProUGUI userName; // Text Mesh Pro 컴포넌트  
	  
	public void SetInformation(string userNameText, Sprite userAvatarSprite)  
	{  
		userName.text = userNameText;  
		userAvatar.sprite = userAvatarSprite;  
	}  
}
```


**LoginPanel.cs**

- 입장 시에 받은 InputField 값을 GameManager에 저장 하도록 코드 변경

```csharp
/// <summary>  
/// 인풋 Text를 이벤트로 받아 글자 수에 따라 검증 조건에 따라 버튼 활성화 여부 확인  
/// </summary>  
/// <param name="value"></param>  
public void OnInputFieldEndEdit(string value)  
{  
	characterName.text = value;  
	entranceBtn.interactable = value.Length is >= 2 and < 10;  
}
```

- Dictionary 타입으로 이름(value)을 Key, 이미지(Sprite)를 value로 Gamemanager로 전달

```csharp
/// <summary>  
/// 인풋 Text를 이벤트로 받아 글자 수에 따라 검증 조건에 따라 버튼 활성화 여부 확인  
/// </summary>  
/// <param name="value"></param>  
public void OnInputFieldEndEdit(string value)  
{  
	characterName.text = value;  
	entranceBtn.interactable = value.Length is >= 2 and < 10; 

	 // GameManager에 이름과 Sprite를 넘겨 줌  
	GameManager.Instance.User = new Dictionary<string, Sprite>
	{  
		{value, characterThumbnail.sprite}  
	};
}
```


**GameManager.cs**

- User의 정보를 `LoginPanel.cs`에서 전달 받으면 해당 데이터를 프로퍼티로 처리
- `OnUser 이벤트`를 `Invoke` 하여 UI 스크립트에서 이를 구독하여 이벤트 실행시 인스턴스를 실행 시킵니다.

```csharp
#region UserInstance Setting  
private Dictionary<string, Sprite> _user = new Dictionary<string, Sprite>(); // 케릭터 이름 변수  
public event Action<Dictionary<string, Sprite>> OnUser; // 이름 세팅 이벤트  
#endregion

/// <summary>  
/// 케릭터 이름 세팅 프로퍼티 및 이벤트 실행  
/// </summary>  
public Dictionary<string, Sprite> User  
{  
	set  
	{  
		_user = value;  
		OnUser?.Invoke(_user);  
	}  
}
```


**SideBar 스크립트**

- GameManager의 `OnUser` 이벤트를 구독하고, 이벤트 발생시 Callback으로 인스턴스화 메서드(`InstantiateUser`)를 실행합니다.
- `InstantiateUser` 메서드의 경우 Callback Params로 Dictionary 자료를 받습니다.
- 받은 데이터는 `Users Class`의 `SetInformation()` 메서드를 실행하여 인스턴스화 및 데이터를 할당합니다.

```csharp
/// <summary>  
/// GameMaanger의 OnUser 이벤트 구독  
/// </summary>  
private void Start()  
{  
	GameManager.Instance.OnUser += InstantiateUser;  
}

/// <summary>  
/// Callback 으로 메서드가 실행되면 Instantiate로 프리팹을 인스턴스화 하고  
/// 이벤트에서 전달 받은 메서드를 Users 인스턴스에 세팅합니다.  
/// </summary>  
/// <param name="user"></param>  
private void InstantiateUser(Dictionary<string, Sprite> user)  
{  
	foreach (var info in user)  
	{  
		Users userInstance = Instantiate(userPrefab, contentTransform);  
		userInstance.SetInformation(info.Key, info.Value);  
	}  
}
```


![](https://i.imgur.com/oUGaUNm.gif)

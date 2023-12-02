---
layout: post
title: 케릭터 Name UI 만들기
subtitle: 케릭터 ID 표시 및 입력 기능 만들기
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
 - TextMeshPro
 - UGUI
banner:
  image: https://i.imgur.com/1TU4tG5.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

케릭터 UGUI 제작
--

### TextMesh Pro Assets 제작

- 대략적인 Range는 해당 블로그 [Text Mesh Pro Create](https://jhoon8903.github.io/unity/2023/10/31/Text-Mesh-Pro.html) 참조
- TMP에서 사용하면서 **한글** 지원이 가능한 Font Asset 만들기
- Font Asset Creator를 선택하여 Asset Editor 실행

![](https://i.imgur.com/EoAY1RO.jpg)

- 사전에 프로젝트에 넣어둔 Font를 할당
- CharacterSet : Custom Range
- Character Sequence 는 `32-126,44032-55203,12593-12643,8200-9900`
- `Generate Font Atlas` 를 서택하여 에셋 생성

![](https://i.imgur.com/CF4dPfn.jpg)

![](https://i.imgur.com/G2PPEBA.jpg)

- 시간이 좀 걸리는 것이 정상이며, 완료되면 다음과 같은 상태를 볼 수 있다

![](https://i.imgur.com/U7bapHW.jpg)

- Save를 눌러 에셋을 생성 및 저장합니다.

![](https://i.imgur.com/HA54TTi.jpg)

- 생성된 에셋을 선택해 Inspector 의 `Atlas Population Mode`를 `Dynamic`으로 변경 합니다.

![](https://i.imgur.com/F3yyMN1.jpg)


### Character ID UGUI

#### 케릭터 Text Object

![](https://i.imgur.com/9MXaefX.jpg)


![](https://i.imgur.com/x7Y61z3.jpg)

- UI 위치를 원하는 위치로 조정합니다.

![](https://i.imgur.com/EuUBlna.jpg)

- 여기서 텍스트 UI 는 계속해서 케릭터 오브젝트를 따라가야 하므로, 케릭터 오브젝트의 자식오브젝트로 위치 시켜주어야 합니다.

![](https://i.imgur.com/RNEL3f2.jpg)


![](https://i.imgur.com/P88yTnE.gif)


### ID Input UGUI

- 하이어라키에 Canvas를 생성
- 해당 캔버스는 모든 UI 및 오브젝트 가장 위에 존재해하므로 Layer 를 생성해줍니다.

![](https://i.imgur.com/tVO0vzA.jpg)

![](https://i.imgur.com/CBQKpXs.jpg)

![](https://i.imgur.com/2jywl54.jpg)

![](https://i.imgur.com/7F5fKs2.jpg)

- Layer를 설정하면 이제부터 `ID_Panel` 레이어는 `Defualt` 레이어의 항상 위에 존재하게 됩니다.

#### Input Field

- ID 입력을 위한 Input Field 를 생성합니다.

![](https://i.imgur.com/OL5aFsz.jpg)

- UI 사이즈에 맞춘 후 Input Field > Text Area 자녀 컴포넌트 확인
- Placeholder 의 경우 화면에 보이는 기본 Default 값을 설정
- Text 에는 유저가 입력한 Text 설정을 합니다.

- 오브젝트 구조는 다음과 같다

![](https://i.imgur.com/fZX76qY.jpg)

- 텍스트를 입력해 봅니다.

![](https://i.imgur.com/AZtjQwV.gif)


### Input Script

- 스크립트를 작성하여 입장 버튼 및 입력한 Input Text 를 케릭터 아이디로 연결합니다.

```csharp
using TMPro;  
using UnityEngine;  
using UnityEngine.UI;  
  
namespace _1.Script.InputText  
{  
	public class InputText : MonoBehaviour  
	{  
		private Image _inputPanel;  
		[SerializeField] private Button entranceBtn;  
		[SerializeField] private TextMeshProUGUI characterName;  
		  
		private void Awake()  
		{  
			_inputPanel = GetComponent<Image>();  
			entranceBtn.onClick.AddListener(ClosePanel);  
		}  
		  
		private void Start()  
		{  
			_inputPanel.gameObject.SetActive(true);  
		}  
		  
		private void ClosePanel()  
		{  
			_inputPanel.gameObject.SetActive(false);  
		}  
		  
		public void OnInputFieldEndEdit(string value)  
		{  
			characterName.text = value;  
		}  
	}  
}
```

- 여기서 `OnInputFieldEndEdit` 는 `Input Field`의 `On End Edit`를 입력 받습니다.
- `On End Edit`의 경우 Input Field 가 입력을 마쳤을때 호출되는 이벤트로 입력받은 string 값을 넘겨 줍니다.

![](https://i.imgur.com/RHdlRHX.jpg)

- 입장 버튼을 눌렀을 때 onClick 클릭 시에 AddListner(Callback Method)를 호출하여 해당 메서드를 호출 합니다.

```csharp
entranceBtn.onClick.AddListener(ClosePanel);
```

![](https://i.imgur.com/1TU4tG5.gif)

### ID Validate

- Text 의 길이가 2 이상 또는 10 이하 에서만 입장할 수 있도록 검증하는 과정이 필요

```csharp
private void Awake()  
{  
	_inputPanel = GetComponent<Image>();  
	entranceBtn.onClick.AddListener(ClosePanel);  
	entranceBtn.interactable = false;  
}

public void OnInputFieldEndEdit(string value)  
{  
	characterName.text = value;  
	entranceBtn.interactable = value.Length is >= 2 and < 10;  
}
```

- Button의 속성 중 하나인 `interactable`의 경우 bool 값으로 버튼 활성화에 대해서 조절 이가능

## Full Script

```csharp
using TMPro;  
using UnityEngine;  
using UnityEngine.UI;  
  
namespace _1.Script.InputText  
{  
	public class InputText : MonoBehaviour  
	{  
		private Image _inputPanel;  
		[SerializeField] private Button entranceBtn;  
		[SerializeField] private TextMeshProUGUI characterName;  
		  
		private void Awake()  
		{  
			_inputPanel = GetComponent<Image>();  
			entranceBtn.onClick.AddListener(ClosePanel);  
			entranceBtn.interactable = false;  
		}  
		  
		private void Start()  
		{  
			_inputPanel.gameObject.SetActive(true);  
		}  
		  
		private void ClosePanel()  
		{  
			_inputPanel.gameObject.SetActive(false);  
		}  
		  
		public void OnInputFieldEndEdit(string value)  
		{  
			characterName.text = value;  
			entranceBtn.interactable = value.Length is >= 2 and < 10;  
		}  
	}  
}
```
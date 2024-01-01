---
layout: post
title: 디자인패턴 (MVC, MVP, MVVM)
subtitle: CS - Design Pattern
author: Daniel
categories: CS
tags: 
 - CS
banner:
  image: https://i.imgur.com/HQxMFLs.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

MVC Pattern
--

- 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 패턴
- Application 구성 요소를 세 가지 역할로 구분
- 각각의 요소에만 집중래서 개발할 수 있음
- 재사용성 및 확장성이 용이

![](https://i.imgur.com/wjNg2rn.jpg)

### 모델
- 데이터 베이스, CharacterData, EnemyData 등

```csharp
using System; 

[Serializable] public class Model 
{ 
	public int score; 
	
	public void AddScore(int points) 
	{ 
		score += points; 
	} 
}
```

### 뷰 
- UI 등 유저의 화면에 보이는 모든 것

```csharp
using UnityEngine; 
using UnityEngine.UI; 

public class View : MonoBehaviour 
{ 
	public Text scoreText; 
	
	public void UpdateScore(int newScore) 
	{ 
		scoreText.text = "Score: " + newScore.ToString(); 
	} 
}
```

### 컨트롤러 
- 뷰와 모델의 상호작용을 관리하는 곳
- 모델과 뷰를 커플링 하는 것이 아닌 컨트롤러를 통해 디커플링 합니다.
- 우리가 흔히 만드는 Manager 클래스가 해당합니다.

```csharp
using UnityEngine; 

public class Controller : MonoBehaviour 
{ 
	public Model model; 
	public View view; 
	
	private void Start() 
	{ 
		view.UpdateScore(model.score); 
	} 
	
	private void Update() 
	{ 
		if (Input.GetKeyDown(KeyCode.Space)) 
		{ 
			view model.AddScore(10); 
			view.UpdateScore(model.score); 
		} 
	} 
}
```

### 장점

- 각 역할에 대한 구분이 확실하여 유지 및 보수성이 뛰어남
- UI를 유지하여 모델만 바꾸어도 작동하며, 모델을 유지하고 UI를 바꿔도 작동합니다.

### 단점
- 설계를 잘못하면 굉장히 복잡해지고 각 기능이 모호해집니다.
- 그로인해 오히려 시간이 더 걸리는 경우가 발생합니다.
- 디렉토리 및 클래스의 갯수가 많아지면서 관리가 힘들어 질 수 있습니다.


#### Unity의 MVC에 적합한 시스템

-  **복잡한 게임 로직**: MVC는 UI와 분리되어야 하는 복잡한 비즈니스 로직이 있는 게임에 적합합니다. 예를 들어 복잡한 시스템을 갖춘 전략 게임이나 RPG는 MVC의 이점을 누릴 수 있습니다.

-  **재사용 가능한 모델이 포함된 게임**: 게임의 로직(모델)을 다양한 플랫폼이나 뷰(예: 모바일 버전 및 PC 버전)에서 사용할 수 있는 경우 MVC는 좋은 구조를 제공합니다.

-  **멀티플레이어 게임**: MVC는 멀티플레이어 게임, 특히 게임 상태 및 플레이어 상호 작용 관리와 관련된 복잡한 논리를 처리하는 데 효과적일 수 있습니다.

-  **UI 중심 게임**: MVC는 본질적으로 UI 관리를 단순화하지는 않지만 UI 로직이 비즈니스 로직과 별도로 유지되는 한 UI가 중요한 구성 요소인 게임을 구성하는 데 도움이 될 수 있습니다.

-  **레벨 편집기 또는 도구 개발**: MVC는 도구의 UI(보기), 도구가 조작하는 데이터(모델) 및 도구를 명확하게 구분하는 레벨 편집기와 같은 게임 내 도구를 개발하는 데 좋은 선택이 될 수 있습니다.

MVC는 애플리케이션 설계를 유연한 아키텍처를 제공하지만 그 이점을 완전히 실현하려면 신중한 구현이 필요합니다. 이는 관심사 분리 및 모듈성에 대한 요구 사항이 잘 정의된 복잡한 애플리케이션에서 가장 효과적입니다.


---

MVP Pattern
--
- Model, View, Presenter 로 구성 된 패턴입니다.
- MVC에서 Controller가 Presenter로 교체 된 패턴 입니다.

![](https://i.imgur.com/vrnN2Mq.jpg)

- View와 Presenter는 `1 : 1` 관계이기 때문에 MVC 패턴보다 강한 결합을 갖습니다.
### 모델

```csharp
public class ScoreModel 
{ 
	public int Score { get; private set; } 
	
	public void AddScore(int points) 
	{ 
		Score += points; 
	} 
}
```

### 뷰

```csharp
using UnityEngine.UI; 

public class ScoreView : MonoBehaviour, IScoreView 
{ 
	public Text scoreText; 
	public Button addScoreButton; 
	public ScorePresenter presenter; 
	
	private void Start() 
	{ 
		presenter = new ScorePresenter(this, new ScoreModel()); 
		addScoreButton.onClick.AddListener(() => presenter.OnAddScoreClicked()); 
	}
	
	public void DisplayScore(int score) 
	{ 
		scoreText.text = $"Score: {score}"; 
	} 
}
```

###  프레젠터

```csharp
public class ScorePresenter 
{ 
	private IScoreView view; 
	private ScoreModel model; 
	
	public ScorePresenter(IScoreView view, ScoreModel model) 
	{ 
		this.view = view; 
		this.model = model; 
		view.DisplayScore(model.Score); 
	} 
	
	public void OnAddScoreClicked() 
	{ 
		model.AddScore(10); 
		view.DisplayScore(model.Score); 
	} 
}
```

### 장점
#### 관심사 분리
- MVP는 프레젠테이션 로직, 비즈니스 로직, 사용자 인터페이스 간의 명확한 분리를 제공하여 코드베이스를 구성하는 데 도움을 주고 읽기 쉽고 유지 관리하기 쉽게 만듭니다.
#### 테스트
- Presenter는 View와 분리되어 있으므로 사용자 인터페이스와 독립적으로 단위 테스트가 가능합니다. 
- 이러한 분리를 통해 비즈니스 논리를 보다 철저하게 테스트할 수 있습니다.
#### 재사용성
- 프레젠터는 필수 뷰 인터페이스를 구현하는 한 다양한 뷰에서 재사용할 수 있습니다. 
- 이렇게 하면 코드의 중복이 줄어들 수 있습니다.
#### 보기 변경의 유연성
- 보기와 발표자가 느슨하게 결합되어 있으므로 발표자에서 최소한의 변경만으로 또는 전혀 변경하지 않고도 사용자 인터페이스를 변경할 수 있습니다.
- 프레젠테이션 로직을 발표자에게 오프로드함으로써 보기는 단순하게 유지되고 사용자 인터페이스 표시에만 집중할 수 있습니다.

### 단점
#### 복잡성
- MVP를 도입하면 아키텍처 복잡성 측면에서 초기 오버헤드가 추가될 수 있습니다. 
- 특히 이러한 추상화 수준이 필요하지 않은 소규모 프로젝트의 경우 더욱 그렇습니다.
#### 상용구 코드 (Boilerplate Code)
- MVP는 모델, 뷰 및 발표자를 위한 인터페이스와 클래스를 만들어야 하므로 상용구 코드의 양이 증가할 수 있습니다.
#### 러닝커브
- 패턴에 익숙하지 않은 개발자는 올바르게 이해하고 구현하는 것이 어려워 패턴의 이점을 활용하지 못하는 부적절한 사용으로 이어질 수 있습니다.
#### 오버헤드
- 레이어(모델, 뷰 및 발표자) 간의 통신은 잘 정의되어야 하며, 
- 이로 인해 간단한 작업에 대한 코드가 더 장황해질 수 있습니다.
#### 프레젠터와 뷰의 커플링
- 프레젠터와 뷰는 개념적으로는 분리되어 있지만 구현에 따라 긴밀하게 결합되어 재사용을 방해할 수 있습니다.
- MVP는 발표자의 명시적인 지시 없이 뷰가 모델 변경에 자발적으로 반응해야 하는 매우 동적인 사용자 인터페이스를 처리할 때 번거로울 수 있습니다.


결론적으로 MVP 패턴은 명확한 분리, 유지 관리 용이성 및 테스트 가능성의 이점이 초기 복잡성과 오버헤드보다 더 큰 중대형 애플리케이션에 유용합니다. 소규모 프로젝트나 UI가 덜 복잡한 프로젝트의 경우 더 단순한 디자인이 더 적합할 수 있습니다.

#### Unity MVP에 적합한 시스템

- **UI 중심 애플리케이션**: MVP는 메뉴 시스템, 캐릭터 사용자 정의 화면 또는 인벤토리 시스템과 같이 사용자 인터페이스에 중점을 두는 애플리케이션에 특히 적합합니다.

- **동적 UI가 포함된 애플리케이션**: 사용자 상호 작용이나 게임 이벤트에 따라 UI를 동적으로 업데이트해야 하는 경우 MVP가 좋은 선택이 될 수 있습니다. Presenter는 Model 변경에 대한 응답으로 View를 업데이트하는 중개자 역할을 할 수 있습니다.

- **모듈형 UI 구성 요소**: 게임의 여러 부분에서 재사용해야 하는 모듈형 UI 구성 요소가 있는 경우 MVP는 여러 발표자가 동일한 보기 인터페이스로 작업할 수 있도록 허용하여 이를 용이하게 할 수 있습니다.

- **간단한 게임 로직**: MVP는 대부분의 복잡성이 UI 레이어에 있고 UI와 기본 데이터 간의 상호 작용이 간단한 간단한 로직을 갖춘 게임에 더 적합할 수 있습니다.

- **UI 테스트 및 프로토타이핑**: MVP는 프레젠테이션 레이어를 로직에서 분리하므로 다양한 UI 디자인과 상호 작용을 빠르게 프로토타이핑하고 테스트하는 데 도움이 될 수 있습니다.

---


MVVM Pattern
--
- MVC의 Contoller가 뷰모델(ViewModel)로 변형 된 패턴

![](https://i.imgur.com/VYlJhTY.jpg)

#### View Model

- ViewModel은 뷰에서 모델을 추상화합니다. 
- 이는 뷰가 모델의 구조나 논리에 대해 아무것도 알 필요가 없음을 의미합니다. 
- 대신, ViewModel은 View가 데이터를 표시하고 기본 비즈니스 로직과 상호 작용하는 데 사용할 수 있는 일련의 데이터 및 명령을 제공합니다.
- MVVM의 주요 기능 중 하나는 View와 ViewModel 간의 데이터 바인딩입니다. 
- View는 ViewModel의 속성에 바인딩되어 모델의 현재 상태를 반영합니다. 
- 모델이 변경되면 ViewModel은 이러한 속성을 업데이트하고 바인딩을 통해 뷰를 자동으로 업데이트합니다.

#### Warning

- Unity에서 MVVM(Model-View-ViewModel) 패턴을 구현하는 것은 WPF 또는 Xamarin과 같은 프레임워크에 비해 약간 까다로울 수 있습니다. 
- Unity는 기본적으로 데이터 바인딩 및 명령을 지원하지 않기 때문입니다. 
- 그러나 어느 정도까지는 MVVM 원칙을 따를 수 있습니다. 다음은 간단한 예입니다

### Model

```csharp
public class PlayerModel 
{ 
	public int Health { get; private set; } 
	
	public PlayerModel(int initialHealth) 
	{ 
		Health = initialHealth; 
	} 
	
	public void TakeDamage(int damage) 
	{ 
		Health -= damage; if (Health < 0) Health = 0; 
	} 
}
```

### View Model

```csharp
using System; 

public class PlayerViewModel 
{ 
	private PlayerModel model; 
	public Action<int> OnHealthChanged; 
	
	public PlayerViewModel(PlayerModel model) 
	{ 
		this.model = model; 
	} 
	
	public void TakeDamage(int damage) 
	{ 
		model.TakeDamage(damage);
		OnHealthChanged?.Invoke(model.Health); 
	} 
}
```

### View

```csharp
using UnityEngine; 
using UnityEngine.UI; 

public class PlayerView : MonoBehaviour 
{ 
	public Text healthText; 
	public Button damageButton; 
	private PlayerViewModel viewModel; 
	
	private void Start() 
	{ 
		var model = new PlayerModel(100); 
		viewModel = new PlayerViewModel(model); 
		viewModel.OnHealthChanged += UpdateHealthDisplay; 
		damageButton.onClick.AddListener(() => viewModel.TakeDamage(10)); 
		UpdateHealthDisplay(model.Health); 
	} 
	
	private void UpdateHealthDisplay(int health) 
	{ 
		healthText.text = $"Health: {health}"; 
	} 
}
```
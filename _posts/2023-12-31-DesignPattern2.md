---
layout: post
title: 디자인패턴 (옵저버, 이터레이터)
subtitle: CS - Design Pattern
author: Daniel
categories: CS
tags: 
 - CS
banner:
  image: https://i.imgur.com/HQxMFLs.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

옵저버 패턴
--

- `Observer Pattern` 주체가 어떤 객체의 상태 변화를 관찰
- 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목로에 있는 옵저버(관찰자)에게 변화를 알려주는 디자인 패턴
- 주체란, 객체의 상태 변화를 보고 있는 관찰자이며, 옵저버들이란 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 `추가 변화 사항`이 생기는 객체들을 의미
- 옵저버 패턴은 주로 이벤트 기반 시스템에 사용
- MVC(Model-View-Controller) 패턴에 사용
#### 옵저버 패턴 종류
- 객체와 주체가 분리되어 있는 옵저버 패턴
- 객체와 주체가 합쳐진 옵저버 패턴

### 옵저버 패턴의 사용 사례

1. **UI 업데이트**: 게임 상태가 변경되면(예: 플레이어 상태, 점수, 인벤토리 업데이트) 이러한 상태를 표시하는 UI 요소는 변경 사항을 관찰하고 그에 따라 업데이트할 수 있습니다.

2. **업적 시스템**: 관찰자는 플레이어 행동이나 게임 상태 변경을 모니터링하여 업적을 잠금 해제할 수 있습니다.

3. **퀘스트 시스템**: 퀘스트는 특정 이벤트나 상태 변경을 관찰하여 퀘스트 단계를 진행하거나 완료할 수 있습니다.

4. **게임 이벤트 시스템**: 예를 들어 플레이어가 특정 지점에 도달하거나 작업을 완료하면 컷신을 트리거합니다.

5. **환경 효과**: 시간이나 날씨 변화에 반응하여 조명을 조정하고, 특정 생물을 생성하거나, 기타 환경 효과를 유발합니다.

6. **AI 상태 변경**: AI 캐릭터는 총소리나 플레이어의 시야와 같은 게임 세계의 변화를 관찰하여 순찰, 추격, 공격 등의 상태를 전환할 수 있습니다.

7. **멀티플레이어 동기화**: 네트워크로 연결된 게임에서는 게임 상태를 동기화된 상태로 유지하기 위해 한 클라이언트의 게임 상태 변경 사항을 다른 클라이언트에서 관찰할 수 있습니다.

8. **난이도 조정**: 플레이어 성능 지표(예: 사망 수 또는 레벨 완료 시간)를 관찰하여 게임 난이도를 동적으로 조정합니다.

### Player 체력 UI 옵저버 패턴

- 체력이 변할 때 플레이어의 UI 체력바를 옵저버 패턴으로 최신화

#### 플레이어 체력

```csharp
public class PlayerHealth : MonoBehaviour 
{ 
	public event Action<float> OnHealthChanged; 
	private float health = 100; 
	public void TakeDamage(float damage) 
	{ 
		health -= damage; OnHealthChanged?.Invoke(health); 
	}
}
```

#### 플레이어 체력 UI

```csharp
public class HealthBar : MonoBehaviour 
{ 
	[SerializeField] private PlayerHealth playerHealth; 
	[SerializeField] private Image healthBarFill; 
	
	private void OnEnable() 
	{ 
		playerHealth.OnHealthChanged += UpdateHealthBar; 
	} 
	
	private void OnDisable() 
	{ 
		playerHealth.OnHealthChanged -= UpdateHealthBar; 
	} 
	
	private void UpdateHealthBar(float newHealth) 
	{ 
		healthBarFill.fillAmount = newHealth / 100; 
	} 
}
```

- 관찰자 패턴은 개체와 밀접하게 결합되지 않고 다른 개체의 변경 사항을 개체에 알려야 하는 시나리오의 게임 개발에서 특히 유용합니다. 
- 이 패턴은 시스템의 한 부분이 다른 부분의 변경에 반응하는 다양한 게임 로직을 구현하는 데 적합합니다. 
- 다음은 Observer 패턴이 적합한 게임 로직의 몇 가지 예입니다.


---


이터레이터 패턴 (iterator pattern)
--
- Iterator 패턴은 기본 표현을 노출하지 않고 집계 개체의 요소에 순차적으로 액세스하는 방법을 제공하는 디자인 패턴입니다. 
- 이 패턴을 사용하면 클라이언트는 컬렉션의 내부 구조를 모르거나 처리하지 않고도 컬렉션의 요소를 탐색할 수 있습니다.

1. **반복자 인터페이스**: 이는 일반적으로 `First()`, `Next()`, `IsDone()` 및 `CurrentItem()`과 같은 메서드를 포함하여 컬렉션을 순회하기 위한 표준 작업을 정의합니다.

2. **Concrete Iterator**: Iterator 인터페이스를 구현하는 클래스이며 반복의 현재 위치를 관리하는 역할을 담당합니다.

3. **집계 인터페이스**: 반복자를 생성하기 위한 작업을 정의합니다. 일반적으로 `CreateIterator()`와 같은 메서드가 있습니다.

4. **Concrete Aggregate**: Aggregate 인터페이스를 구현하고 객체 컬렉션을 포함하는 클래스입니다. 해당 요소에 액세스하고 순회하기 위해 Concrete Iterator의 인스턴스를 반환합니다.

Iterator 패턴의 주요 이점은 다음과 같습니다.

- **순회 논리의 추상화**: 클라이언트는 컬렉션을 순회하기 위해 컬렉션의 데이터 구조의 복잡성을 이해할 필요가 없습니다.
- **반복 로직에서 컬렉션 분리**: 컬렉션 클래스와 반복 로직이 분리되어 반복 로직을 변경하지 않고도 새로운 유형의 컬렉션을 더 쉽게 추가할 수 있습니다.
- **다중 순회**: 여러 반복자가 동시에 동일한 컬렉션을 독립적으로 순회할 수 있습니다.


### 이터레이터 패턴 사용 사례

- Iterator 패턴은 게임 개발에서 객체 컬렉션을 처리하거나 특정 작업을 순차적으로 수행하는 데 효과적으로 사용될 수 있습니다.

1. **인벤토리 관리**: 플레이어의 인벤토리를 반복하여 특정 항목을 찾고, 항목 상태를 업데이트하거나, UI에 항목을 표시합니다.

2. **AI 순찰 경로**: AI 캐릭터의 웨이포인트 목록을 관리합니다. 각 AI 캐릭터는 반복기를 사용하여 순찰 경로를 따라 이동하고 각 웨이포인트를 순서대로 방문할 수 있습니다.

3. **턴 기반 시스템**: 턴 기반 게임에서는 반복자를 사용하여 각 플레이어나 NPC의 턴을 순환할 수 있습니다.

4. **스폰 관리**: 스폰 지점 목록이 있는 경우 반복기를 사용하여 이를 통해 적, 파워업 또는 기타 개체를 체계적으로 스폰할 수 있습니다.

5. **대화 시스템**: 분기형 대화 또는 대화 트리가 있는 게임의 경우 반복자는 일련의 대화 프롬프트를 통해 진행을 관리할 수 있습니다.

6. **퍼즐 게임**: 퍼즐 조각이나 요소 세트를 반복하고, 상태를 확인하고, 플레이어가 게임과 상호 작용할 때 업데이트합니다.

7. **데미지 처리**: 이벤트로 인해 여러 객체(예: 전략 게임의 유닛)가 데미지를 입는 경우 반복자는 각 유닛에 순차적으로 데미지를 가할 수 있습니다.

8. **환경 상호 작용**: 환경의 모든 상호 작용 요소를 반복하여 플레이어 작업이나 기타 이벤트에 따라 상태를 확인하고 업데이트합니다.

9. **자원 수집**: 전략 또는 생존 게임의 경우 모든 리소스 노드를 반복하여 사용 가능한 리소스를 업데이트하거나 수집합니다.

10. **효과 및 애니메이션 순서 지정**: 일련의 개체에 시각적 효과나 애니메이션을 순서대로 적용합니다.

11. **충돌 감지**: 어떤 경우에는 특히 간단한 2D 게임에서 반복자를 사용하여 객체 간의 충돌을 확인할 수 있습니다.

12. **레벨 세그먼트 활성화/비활성화**: 플레이어가 게임을 진행하면서 레벨 세그먼트(예: 무한 러너)를 활성화하거나 비활성화합니다.

### 플레이어 턴제 이터레이터 패턴 예시

- 각 플레이어가 차례로 차례를 갖습니다. 
- `TurnManager`는 반복자를 사용하여 플레이어 목록을 순환하고 끝에 도달하면 시작 부분으로 다시 반복됩니다. 
- 이는 각 플레이어에게 차례가 있고 플레이 순서가 유지되도록 하는 간단한 방법입니다.

```csharp
public class TurnManager : MonoBehaviour 
{ 
	private List<Player> players; 
	private IIterator<Player> turnIterator; 
	
	private void Start() 
	{ 
		players = new(){ 반복할 플레이어 리스트 }
		ConcreteAggregate<Player> playerAggregate = new(players); 
		turnIterator = playerAggregate.CreateIterator(); 
		StartTurn(); 
	} 
	
	private void StartTurn() 
	{ 
		if (!turnIterator.HasNext()) 
		{ 
			// 마지막 플레이어에 도달하면 처음 플레이어로 리셋
			turnIterator.Reset(); 
		} 
		var currentPlayer = turnIterator.Next(); 
		currentPlayer.StartTurn(); 
	} 
	
	public void EndTurn() 
	{ 
		// 현재 플레이어의 턴이 끝나면 다음 플레이어 턴 시작
		StartTurn(); 
	} 
} 
```

```csharp
public class Player : MonoBehaviour 
{ 
	public void StartTurn() 
	{ 
		// 플레이어의 턴이 시작할때의 로직
	} 
}
```


---
layout: post
title: 디자인패턴 (싱글턴, 팩토리, 전략패턴)
subtitle: CS - Design Pattern
author: Daniel
categories: CS
tags: 
 - CS
banner:
  image: https://i.imgur.com/HQxMFLs.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

디자인 패턴
--
- 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계등을 이용하여 해결할 수 있도록 하나의 `규약`형태로 만들어 놓은 것

싱글톤 패턴 (Singleton Pattern)
--
- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는 데 쓰이며, 보통 데이터베이스 연결 모듈에 많이 사용
### Singleton 인스턴스 생성

#### 프로퍼티를 이용한 싱글턴 인스턴스 생성

```csharp
private static GameManager _instence;
private static bool _initialized;

public static GameManager Instance
{
	get
	{
		if (_initialized) return _instance;
		_initialized = true;

		GameObject gameManager = GameObject.Find("GameManager");
		if (gameManager != null) return _intance;

		gameManager = new () {name = "GameManager"};
		gameManager.AddComponent<GameManager>();
		DonDestroyOnLoad(gameManager);
		_intance = gameManager.GetComponent<GameManager>();
		return _instance;
	}
}
```

#### 초기화시 싱글턴 인스턴스를 생성

```csharp
public static GameManager Instance;

private void Awake()
{
	if(Instance == null)
	{
		Instance = this;
		DonDestroyOnLoad(gameObject);
	}
	else 
	{
		Destroy(gameObjet);
	}
}
```

#### 장점
- 하나의 인스턴스를 생성할 때 드는 비용이 줄어듬
#### 단점
- 의존성이 높아짐
	- 의존성 주입(DI, Dependency Injection)으로 모듈 간의 결합을 좀 더 느슨하게 해결이 가능해짐
	-  메인 모듈이 `직접` 하위 모듈에 대한 의존성을 주기 보다는 `의존성 주입자`가 이부분을 가로채 메인 모듈이 `간접`적으로 의존성을 주입하는 방식
	- 이를 통해 상위 모듈은 하위 모듈에 대한 의존성이 떨어지게 됨(`디커플링`)
	- 다만 시스템적인 복잡성이 늘어나며, 런타임 패널티가 생길 수 있음
	
![](https://i.imgur.com/X3dxP8D.jpg)


- TDD를 할 때 단위 테스트 시 서로 독립적으로 실행 되어야 하지만, 싱글턴 패턴의 경우 `독립적인` 인스턴스 생성이 어려움

---

팩토리 패턴 (Factory Pattern)
--
- 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
- 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 대한 구체적인 내용을 결정하는 패턴
- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며, 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 됨
- 객체 생성 로직이 따로 떼어져 있기 떄문에 코드를 리팩토링 하더라도 필요한 한부분만 고치면 되어 유지 보수성이 증가함
- 팩토리 패턴은 런타임까지 객체의 정확한 유형을 알 수 없는 객체를 생성하는 데 유용합니다.
### 팩토리 패턴을 이용한 몬스터 생성

#### 몬스터 공통 추상 클래스

```csharp
public abstract class Monster 
{ 
	public abstract void Attack(); 
}
```

#### 몬스터 하위 클래스

```csharp
public class Goblin : Monster 
{ 
	public override void Attack() 
	{ 
		Debug.Log("Goblin attacks!"); 
	} 
} 

public class Orc : Monster 
{ 
	public override void Attack() 
	{ 
		Debug.Log("Orc attacks!"); 
	} 
}
```

#### 몬스터를 팩토리 클래스 (상위 클래스)

```csharp
public static class MonsterFactory 
{ 
	public static Monster CreateMonster(string type) 
	{ 
		switch (type) 
		{ 
			case "Goblin": 
				return new Goblin(); 
			case "Orc": 
				return new Orc(); 
			default: 
				throw new ArgumentException("Invalid monster type"); 
		} 
	} 
}
```

#### 몬스터 생성 클래스

```csharp
public class GameController : MonoBehaviour 
{ 
	private void Start() 
	{ 
		Monster goblin = MonsterFactory.CreateMonster("Goblin");
		goblin.Attack(); 
		
		Monster orc = MonsterFactory.CreateMonster("Orc");
		orc.Attack(); 
	} 
}
```

---

전략패턴
--

- 전략 • 정책 패턴 이라고 하며, 객체의 행위를 바꾸고 싶은 경우 `직접` 수정하지 않고 전략이라고 부르는 `캡슐화한 알고리즘`을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴

### 전략 패턴을 이용한 몬스터 생성 

#### 몬스터 생성 전략 인터페이스

```csharp
public interface IMonsterCreationStrategy 
{ 
	Monster CreateMonster(); 
}
```

#### 구체적인 전략 구현

```csharp
public class GoblinCreationStrategy : IMonsterCreationStrategy 
{ 
	public Monster CreateMonster() 
	{ 
		return new Goblin(); 
	} 
} 

public class OrcCreationStrategy : IMonsterCreationStrategy 
{ 
	public Monster CreateMonster() 
	{ 
		return new Orc(); 
	} 
}
```

#### MonsterFactory 리팩터링

```csharp
public static class MonsterFactory 
{ 
	private static Dictionary<string, IMonsterCreationStrategy> _creationStrategies = new (); 
	{ 
		{ "Goblin", new GoblinCreationStrategy() }, 
		{ "Orc", new OrcCreationStrategy() } 
	}; 
	
	public static Monster CreateMonster(string type) 
	{ 
		if (_creationStrategies.TryGetValue(type, out IMonsterCreationStrategy strategy)) 
		{ 
			return strategy.CreateMonster(); 
		} 
		throw new ArgumentException("Invalid monster type"); 
	} 
}
```

#### 몬스터 생성 클래스

```csharp
public class GameController : MonoBehaviour 
{ 
	private void Start() 
	{ 
		Monster goblin = MonsterFactory.CreateMonster("Goblin"); goblin.Attack();
		Monster orc = MonsterFactory.CreateMonster("Orc"); orc.Attack(); 
	} 
}
```

- `IMonsterCreationStrategy` 인터페이스는 몬스터를 생성하는 방법을 정의합니다.
- 각 특정 몬스터 유형('Goblin' 및 'Orc' 등)에는 'IMonsterCreationStrategy'를 구현하는 해당 전략 클래스가 있습니다.
- 'MonsterFactory'는 이제 몬스터 유형을 생성 전략에 매핑하는 사전을 보유합니다. 이러한 전략을 사용하여 몬스터 인스턴스를 생성합니다.
- 이 접근 방식을 사용하면 스위치 문이나 if-else 체인이 필요하지 않습니다. 
- 새로운 몬스터 유형을 추가하는 것은 새로운 전략 클래스를 생성하고 이를 공장 사전에 등록하는 것만큼 간단합니다.
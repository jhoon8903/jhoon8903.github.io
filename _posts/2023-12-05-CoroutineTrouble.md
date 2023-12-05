---
layout: post
title: Coroutine NullReference Exception
subtitle: 
author: Daniel
categories: Unity
tags: 
 - Unity
 - Programming
 - Coroutine
banner:
  image: https://i.imgur.com/mYEIGJo.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

StartCoroutine NullReference 에러
--

### 문제 발생

- 적의 이동 패턴 구현 중 MoveToward 방식의 움직임에서 `Coroutine`에 문제가 발생

![](https://i.imgur.com/7hz7Ab8.jpg)

![](https://i.imgur.com/ctoXbBo.jpg)

- 처음에는 `enemy`, `wayPoint`가 문제가 있는지 확인
- 확인 방법은 `Debug.Log()` 및 IDE의 `Debug`를 활용
- 모든 데이터에는 문제가 없는 것으로 확인 됨

#### IDE Debug 내용

- 해당 `Breaking Point`를 확인해도 정상적으로 데이터가 들어와 있는 것을 확인

![](https://i.imgur.com/Ge0szcc.jpg)

### 해결방법

- Unity Manual : [NullReferenceException on StartCoroutine()]((https://discussions.unity.com/t/nullreferenceexception-on-startcoroutine/110322)
- OverStackFlow : [StartCoroutine gives NullReferenceException](https://stackoverflow.com/questions/60969942/startcoroutine-gives-nullreferenceexception)
- GameDevelopment : [NullReferenceException in StartCoroutine method](https://gamedev.stackexchange.com/questions/133512/nullreferenceexception-in-startcoroutine-method)

#### StartCoroutine의 작동 방법

- StartCoroutine을 작동 시키기 위해서는 **반드시 게임 컴포넌트에 붙어있어야한다** 입니다.
- StartCoroutine은 게임오브젝트가 한 프레임에서 다음 프레임으로 넘어갈때 `MonoBehaviour`를 추적하게 되는데 이때 코루틴과 게임 오브젝트간에 관계를 등록하려고 합니다.
- 이는 게임 오브젝트가 파괴되거나 비활성화될 경우, 해당 코루틴이 중단되고 메모리 누수가 발생하지 않도록 하기 위해서 입니다.
- 만약 게임 오브젝트를 찾지 못하면 널 참조 예외가 발생합니다.

![](https://i.imgur.com/mYEIGJo.jpg)

- 해당 Pattern Script에서는 오브젝트를 받아오기만 할 뿐 실제 `Enemy` 객체의 `MonoBehaviour`를 추적하지 못하기 때문에 NullReference가 발생하는것으로 판단
- 해당 IEnumerator 는 Pattern 스크립트에서 작성하고, 실제 코루틴에 대한 실행은 
- MonoBehaviour 객체화 된 Enemy 객체에서 실행하는 것으로 코드를 변경하였습니다.


### 기존 코드

#### EnemySpawn.cs

```csharp
private void StartZigzagMovement(Enemy enemy)  
{  
	Vector2[] wayPoints = Main.Spawn.CalculateZigzagWaypoints(enemy);  
	Coroutine coroutine = StartCoroutine(MoveAlongWaypoints(enemy, wayPoints));  
}  
  
public Vector2[] CalculateZigzagWaypoints(Enemy enemy)  
{  
	int direction = Random.Range(0, 2) == 0 ? -1 : 1;  
	Vector2[] wayPoints = new Vector2[5];  
	Vector2 startPosition = enemy.transform.position;  
	  
	for (int i = 0; i < wayPoints.Length; i++)  
	{  
		float nextX = Mathf.Clamp(startPosition.x + direction, -4f, 4f);  
		float nextY = Mathf.Clamp(startPosition.y - 2 * (i + 1), -5f, 5f);  
		wayPoints[i] = new Vector2(nextX, nextY);  
		direction *= -1;  
		startPosition = wayPoints[i];  
	}  
	return wayPoints;  
}  
  
private static IEnumerator MoveAlongWaypoints(Enemy enemy, Vector2[] wayPoints)  
{  
	int currentWaypointIndex = 0;  
	while (currentWaypointIndex < wayPoints.Length)  
	{  
		Vector2 currentWaypoint = wayPoints[currentWaypointIndex];  
		while (Vector2.Distance(enemy.transform.position, currentWaypoint) > 0.01f)  
		{  
			enemy.transform.position = Vector2.MoveTowards(  
			enemy.transform.position,  
			currentWaypoint,  
			enemy.speed * Time.deltaTime);  
			yield return null;  
		}  
		currentWaypointIndex++;  
	}  
	Main.Object.Despawn(enemy);  
}
```


### 변경 코드

#### Enemy.cs

```csharp
#region MoveMentPattern  
/// <summary>  
/// 지그재그패턴 총 3번의 움직임 변화가 있음 (파라매터로 전달)  
/// </summary>  
public void Zigzag()  
{  
	Vector2[] wayPoints = Main.Spawn.CalculateZigzagWaypoints(this, 3);  
	Coroutine coroutine = StartCoroutine(Main.Spawn.MoveAlongWaypoints(this, wayPoints));  
}  
#endregion
```

#### EnemySpawn.cs

```csharp
private void EnemyPattern(List<Enemy> enemiesList)  
{  
	foreach (Enemy enemy in enemiesList)  
	{  
		enemy.gameObject.SetActive(true);  
		  
		switch (enemy.movePattern)  
		{  
			case Pattern.VERTICAL:  
				enemy.Vertical();  
				break;  
			case Pattern.ZIGZAG:  
				enemy.Zigzag();  
				break;  
			case Pattern.HORIZONTAL:  
				enemy.Horizontal();  
				break;  
		}  
	}  
}  
  
public Vector2[] CalculateZigzagWaypoints(Enemy enemy, int wayPointCount)  
{  
	int direction = Random.Range(0, 2) == 0 ? -1 : 1;  
	Vector2[] wayPoints = new Vector2[wayPointCount];  
	Vector2 startPosition = enemy.transform.position;  
	  
	for (int i = 0; i < wayPoints.Length; i++)  
	{  
		float nextX = Mathf.Clamp(startPosition.x + direction, -4f, 4f);  
		float nextY = Mathf.Clamp(startPosition.y - 2 * (i + 1), -5f, 5f);  
		wayPoints[i] = new Vector2(nextX, nextY);  
		direction *= -1;  
		startPosition = wayPoints[i];  
	}  
	return wayPoints;  
}  
  
public IEnumerator MoveAlongWaypoints(Enemy enemy, Vector2[] wayPoints)  
{  
	int currentWaypointIndex = 0;  
	while (currentWaypointIndex < wayPoints.Length)  
	{  
		Vector2 currentWaypoint = wayPoints[currentWaypointIndex];  
		while (Vector2.Distance(enemy.transform.position, currentWaypoint) > 0.01f)  
		{  
			enemy.transform.position = Vector2.MoveTowards(  
			enemy.transform.position,  
			currentWaypoint,  
			enemy.speed * Time.deltaTime);  
			yield return null;  
		}  
		currentWaypointIndex++;  
	}  
	Main.Object.Despawn(enemy);  
}
```


마치며
--

코루틴을 사용할때 그냥 버릇처럼 MonoBehaviour를 상속받는 오브젝트에서 사용해서 처음겪는 문제였습니다. 해결이 늦어진 이유는 `Coroutine`의 정확한 실행 방식에 대한 무지였고, 이 문제는 아무생각없이 코루틴을 사용해왔기 떄문입니다.     

이제는 StartCoroutine의 실행 방법에 대해 알게 된 만큼 사용함에 있어 좀 더 정확한 방법으로 정확한 위치에 사용이 가능할 것 입니다.
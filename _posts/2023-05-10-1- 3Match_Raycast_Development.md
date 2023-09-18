---
title: 3 Match RayCast
category: Unity
author: 이정훈
tags:
  - game
  - unity
img: https://i.imgur.com/9e68Cnv.gif
comments_disable: true
meta_description: 3 Match RayCast
---


![](https://i.imgur.com/9e68Cnv.gif)
# Unity 이동 및 병합기능 만들기

안녕하세요! 오늘은 Unity를 사용해서 3-Match 퍼즐 게임에서 캐릭터를 이동시키는 간단한 로직을 공유하려고 합니다. 저도 이제 막 Unity를 시작한 초보 개발자라서, 함께 배워가면서 이런 블로그를 작성하게 되었어요. 

코드들은 매일 수정되어 업데이트 될 예정입니다.

## 코드 소개

```csharp
using UnityEngine;  
using UnityEngine.EventSystems;  
using System.Collections;  
  
public class MovementManager : MonoBehaviour  
{  
public float moveDuration = 0.3f;  
public CountManager countManager;  
public float hoverScaleFactor = 1.1f;  
  
private GameObject selectedCharacter;  
private Vector3 originalScale;  
  
private void Update()  
{  
//if (EventSystem.current.IsPointerOverGameObject()) return;  
  
	if (Input.GetMouseButtonDown(0))  
	{  
		Vector3 MousePos = Camera.main.ScreenToWorldPoint(Input.mousePosition);  
		RaycastHit2D hit;  
		int layerMask = LayerMask.GetMask("Character");  
		Debug.Log(MousePos);  
		hit = Physics2D.Raycast(MousePos, transform.forward, 10f, layerMask);  
		if (hit.collider != null)  
		{  
			Debug.Log("Click");  
			if (hit.collider.gameObject.CompareTag("Character") 
			&& countManager.CanMove())  
			{  
				selectedCharacter = hit.collider.gameObject;  
				originalScale = selectedCharacter.transform.localScale;  
				selectedCharacter.transform.localScale *= hoverScaleFactor;  
			}  
		}  
	}  
  
	if (Input.GetMouseButtonUp(0) && selectedCharacter != null)  
	{  
		selectedCharacter.transform.localScale = originalScale;  
		  
		Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);  
		RaycastHit hit;  
		if (Physics.Raycast(ray, out hit, Mathf.Infinity))  
		{  
			GameObject targetCharacter = hit.collider.gameObject;  
			if (targetCharacter.CompareTag("Character") 
			&& CanMove(selectedCharacter.transform.position, 
			targetCharacter.transform.position))  
			{  
				StartCoroutine(SwapCharacters(selectedCharacter, 
				targetCharacter));  
				countManager.DecreaseMoveCount();  
			}  
		}  
		selectedCharacter = null;  
	}  
}  
  
private bool CanMove(Vector3 pos1, Vector3 pos2)  
{  
	float distance = Vector3.Distance(pos1, pos2);  
	return distance <= 1.5f && countManager.CanMove();  
}  
  
public IEnumerator SwapCharacters(GameObject character1, GameObject character2)  
{  
	Vector3 initialPos1 = character1.transform.position;  
	Vector3 initialPos2 = character2.transform.position;  
  
	float elapsedTime = 0f;  
  
	while (elapsedTime < moveDuration)  
	{  
		elapsedTime += Time.deltaTime;  
  
		character1.transform.position = 
		Vector3.Lerp(initialPos1, initialPos2, elapsedTime / moveDuration);  
		character2.transform.position = 
		Vector3.Lerp(initialPos2, initialPos1, elapsedTime / moveDuration);  
  
	yield return null;  
	}  
  
	character1.transform.position = initialPos2;  
	character2.transform.position = initialPos1;  
  
	Debug.Log($"Moved character to ({initialPos2.x}, {initialPos2.y})");  
	}  
}
```

여기서 사용된 네임스페이스와 클래스는 Unity에서 제공하는 기본적인 것들이에요.

### 변수들

- `moveDuration`: 캐릭터가 이동하는 데 걸리는 시간입니다.
- `countManager`: 캐릭터를 몇 번 움직일 수 있는지 관리하는 매니저입니다.
- `hoverScaleFactor`: 마우스로 캐릭터를 선택했을 때 커지는 배율입니다.

### 마우스 이벤트 처리

- `Input.GetMouseButtonDown(0)`: 마우스 왼쪽 버튼을 눌렀을 때의 로직. 
  캐릭터를 선택하면 그 캐릭터는 조금 커집니다.
- `Input.GetMouseButtonUp(0)`: 마우스 왼쪽 버튼을 뗐을 때의 로직. 
  선택한 캐릭터를 다른 위치로 이동시킵니다.

### 이동 가능 여부 확인

- `CanMove()`: 두 캐릭터의 위치가 주어진 범위 내에 있는지와 이동 가능한 횟수가 남아 있는지 확인합니다.

### 캐릭터 스왑

- `SwapCharacters()`: 두 캐릭터의 위치를 스왑하는 코루틴입니다.

## 마치며

Unity로 간단한 3-Match 퍼즐 게임의 이동 로직을 만들어 보았습니다. 
아직 많이 부족하지만 함께 공부하면서 발전해 나가려고 합니다. 
조언이나 피드백은 언제나 환영입니다!
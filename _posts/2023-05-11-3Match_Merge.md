---
title: 3Match_Merge 시작
category: game
author: 이정훈
tags:
  - unity
  - game
  - c#
img: https://i.imgur.com/nkZIvoY.gif
comments_disable: true
meta_description: 3 Match Merge
---


![](https://i.imgur.com/nkZIvoY.gif)
# Unity에서 3-Match 퍼즐의 병합 로직 이해하기

안녕하세요! 이번에는 Unity에서 3-Match 퍼즐 게임의 병합 로직을 어떻게 구현하는지 공유하려고 합니다. 아직 완벽하게 작동하지 않는 부분이 있어서, 이 부분에 대한 피드백이나 제안도 환영합니다!

## MergeManager 클래스 소개

```csharp
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
  
public class MergeManager : MonoBehaviour  
{  
	[SerializeField] private GridManager gridManager;  
	[SerializeField] private float mergeDelay = 0.5f;  
	  
	public bool CheckForMatches(GameObject character)  
	{  
		int horizontalMatches = CountMatches(character, Vector2.right) 
		+ CountMatches(character, Vector2.left) + 1;  
		int verticalMatches = CountMatches(character, Vector2.up) 
		+ CountMatches(character, Vector2.down) + 1;  
		  
		if (horizontalMatches >= 3 || verticalMatches >= 3)  
		{  
			Debug.Log("Merging detected");  
			if (horizontalMatches >= 3)  
			{  
				Debug.Log("Merging horizontally");  
				StartCoroutine(MergeCharacters(character, Vector2.right,
				horizontalMatches - 1));  
				StartCoroutine(MergeCharacters(character, Vector2.left, 
				horizontalMatches - 1));  
			}  
		  
			if (verticalMatches >= 3)  
			{  
				Debug.Log("Merging vertically");  
				StartCoroutine(MergeCharacters(character, Vector2.up, 
				verticalMatches - 1));  
				StartCoroutine(MergeCharacters(character, Vector2.down, 
				verticalMatches - 1));  
			}  
		  
			return true;  
		}  
		return false;  
	}  
  
	private int CountMatches(GameObject character, Vector2 direction)  
	{  
		int matchCount = 0;  
		Vector2 currentPosition = character.transform.position;  
		string characterPrefabName = character.transform.parent.name.Replace
		("(Clone)", "").Trim();  
		  
		while (true)  
		{  
			currentPosition += direction;  
			GameObject otherCharacter = 
			gridManager.GetCharacterAtPosition(currentPosition);  
			  
			if (otherCharacter != null)  
			{  
				string otherCharacterPrefabName = 
				otherCharacter.transform.name.Replace("(Clone)", "").Trim();  
				if (otherCharacterPrefabName == characterPrefabName 
				&& ShouldConsiderForMatching(otherCharacter))  
				{  
					matchCount++;  
				}  
				else  
				{  
					break;  
				}  
			}  
			else  
			{  
				break;  
			}  
		}  
		return matchCount;  
	}  
  
	private bool ShouldConsiderForMatching(GameObject character)  
	{  
		return true;  
	}  
  
	private IEnumerator MergeCharacters(GameObject character, Vector2 direction, 
	int count)  
	{  
		Vector2 currentPosition = character.transform.position;  
		string characterPrefabName = character.transform.parent.name.Replace
		("(Clone)", "").Trim();  
	  
		for (int i = 0; i < count; i++)  
		{  
			currentPosition += direction;  
			GameObject otherCharacter = 
			gridManager.GetCharacterAtPosition(currentPosition);  
		  
			if (otherCharacter != null)  
			{  
				string otherCharacterPrefabName =
				otherCharacter.transform.name.Replace("(Clone)", "").Trim();  
				if (otherCharacterPrefabName == characterPrefabName 
				&& ShouldConsiderForMatching(otherCharacter))  
				{  
					gridManager.RemoveCharacterAtPosition(currentPosition);  
					Destroy(otherCharacter);  
				}  
			}  
		}  
		yield return new WaitForSeconds(mergeDelay);  
	}  
}
```

### 변수 설명

- `gridManager`: 그리드를 관리하는 매니저입니다.
- `mergeDelay`: 병합 후의 딜레이 시간입니다.

### 병합 가능 여부 확인하기

```csharp
public bool CheckForMatches(GameObject character)  
	{  
		int horizontalMatches = CountMatches(character, Vector2.right) 
		+ CountMatches(character, Vector2.left) + 1;  
		int verticalMatches = CountMatches(character, Vector2.up) 
		+ CountMatches(character, Vector2.down) + 1;  
		  
		if (horizontalMatches >= 3 || verticalMatches >= 3)  
		{  
			Debug.Log("Merging detected");  
			if (horizontalMatches >= 3)  
			{  
				Debug.Log("Merging horizontally");  
				StartCoroutine(MergeCharacters(character, Vector2.right,
				horizontalMatches - 1));  
				StartCoroutine(MergeCharacters(character, Vector2.left, 
				horizontalMatches - 1));  
			}  
		  
			if (verticalMatches >= 3)  
			{  
				Debug.Log("Merging vertically");  
				StartCoroutine(MergeCharacters(character, Vector2.up, 
				verticalMatches - 1));  
				StartCoroutine(MergeCharacters(character, Vector2.down, 
				verticalMatches - 1));  
			}  
		  
			return true;  
		}  
		return false;  
	}  
```
이 함수는 주어진 캐릭터를 중심으로 연속된 캐릭터들을 찾아 병합 여부를 확인합니다.

### 연속된 캐릭터 수 세기

```csharp
	private int CountMatches(GameObject character, Vector2 direction)  
	{  
		int matchCount = 0;  
		Vector2 currentPosition = character.transform.position;  
		string characterPrefabName = character.transform.parent.name.Replace
		("(Clone)", "").Trim();  
		  
		while (true)  
		{  
			currentPosition += direction;  
			GameObject otherCharacter = 
			gridManager.GetCharacterAtPosition(currentPosition);  
			  
			if (otherCharacter != null)  
			{  
				string otherCharacterPrefabName = 
				otherCharacter.transform.name.Replace("(Clone)", "").Trim();  
				if (otherCharacterPrefabName == characterPrefabName 
				&& ShouldConsiderForMatching(otherCharacter))  
				{  
					matchCount++;  
				}  
				else  
				{  
					break;  
				}  
			}  
			else  
			{  
				break;  
			}  
		}  
		return matchCount;  
	}  
```
이 함수는 주어진 방향으로 연속된 캐릭터의 수를 반환합니다.

### 캐릭터 병합하기

이 함수는 연속된 캐릭터들을 병합합니다.

```csharp
private IEnumerator MergeCharacters(GameObject character, Vector2 direction, 
	int count)  
	{  
		Vector2 currentPosition = character.transform.position;  
		string characterPrefabName = character.transform.parent.name.Replace
		("(Clone)", "").Trim();  
	  
		for (int i = 0; i < count; i++)  
		{  
			currentPosition += direction;  
			GameObject otherCharacter = 
			gridManager.GetCharacterAtPosition(currentPosition);  
		  
			if (otherCharacter != null)  
			{  
				string otherCharacterPrefabName =
				otherCharacter.transform.name.Replace("(Clone)", "").Trim();  
				if (otherCharacterPrefabName == characterPrefabName 
				&& ShouldConsiderForMatching(otherCharacter))  
				{  
					gridManager.RemoveCharacterAtPosition(currentPosition);  
					Destroy(otherCharacter);  
				}  
			}  
		}  
		yield return new WaitForSeconds(mergeDelay);  
	}  
```

아직 버그가 많은 초기 코드입니다,

참고하시기에는 무리가 있으며, 계속 업데이트 될 예정입니다.


---
title: 3Match Grid Proto Type
category: Unity
author: 이정훈
tags:
  - unity
  - c#
  - game
img: https://i.imgur.com/9e68Cnv.gif
comments_disable: true
meta_description: Unity 3 Match Grid
---

# Unity로 3-Match 퍼즐의 그리드 생성하기

안녕하세요! 지난 포스팅에서 3-Match 퍼즐의 이동 로직을 다뤘는데요, 이번에는 그리드를 생성하고 캐릭터를 배치하는 방법에 대해 알아보려고 합니다. 아직 Unity 초보라서 간단하게만 작성해봤어요. 함께 보시죠! 😄

## GridManager 클래스 소개

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GridManager : MonoBehaviour
{
    public int gridHeight = 6;
    public GameObject row1Prefab;
    public GameObject row2Prefab;
    public CharacterManager characterManager;
    public CharacterPool characterPool;

    private int currentRow = 1;

    public IEnumerator GenerateInitialGrid()
    {
        for (int y = 0; y < gridHeight; y++)
        {
            GameObject newRow = GenerateRow(new Vector3(0, 1 - y, 0));
            currentRow = (currentRow == 1) ? 2 : 1;
        }

        yield return new WaitForSeconds(1.0f);
        foreach (Transform rowTransform in transform)
        {
            SpawnCharactersInRow(rowTransform);
        }
    }

    public void AddRow() // row생성 호출 시 추가 row 생성
    {
        gridHeight++;
        GameObject newRow = GenerateRow(new Vector3(0, -gridHeight + 1, 0));
        SpawnCharactersInRow(newRow.transform);
        currentRow = (currentRow == 1) ? 2 : 1;
    }

    private GameObject GenerateRow(Vector3 position)
    {
        GameObject rowPrefab = (currentRow == 1) ? row1Prefab : row2Prefab;
        return Instantiate(rowPrefab, position, Quaternion.identity, transform);
    }

    private void SpawnCharactersInRow(Transform rowTransform)
    {
        for (int i = 0; i < rowTransform.childCount; i++)
        {
            Transform gridTransform = rowTransform.GetChild(i);
            GameObject characterPrefab = 
            characterManager.GetRandomCharacterPrefab();
            Vector3 newPosition = new Vector3(gridTransform.position.x, 
            gridTransform.position.y, -0.5f);
            GameObject characterInstance = 
            characterPool.GetPooledObject(characterPrefab);
            characterInstance.transform.position = newPosition;
            characterInstance.transform.SetParent(gridTransform, true);
            characterInstance.SetActive(true);

            // Disable all child objects except for the one named "level0"
            for (int j = 0; j < characterInstance.transform.childCount; j++)
            {
                Transform child = characterInstance.transform.GetChild(j);
                if (child.name != "level0")
                {
                    child.gameObject.SetActive(false);
                }
            }
        }
    }
}
```

이 클래스는 게임 보드의 그리드를 관리합니다.

### 변수 설명

- `gridHeight`: 그리드의 초기 높이입니다.
- `row1Prefab`, `row2Prefab`: 두 종류의 행 프리팹입니다.
- `characterManager`: 랜덤 캐릭터를 가져오는 역할을 합니다.
- `characterPool`: 사용 가능한 캐릭터 오브젝트 풀입니다.
- `currentRow`: 현재 생성 중인 행의 번호입니다.

### 그리드 생성하기

```csharp
public IEnumerator GenerateInitialGrid()
    {
        for (int y = 0; y < gridHeight; y++)
        {
            GameObject newRow = GenerateRow(new Vector3(0, 1 - y, 0));
            currentRow = (currentRow == 1) ? 2 : 1;
        }

        yield return new WaitForSeconds(1.0f);
        foreach (Transform rowTransform in transform)
        {
            SpawnCharactersInRow(rowTransform);
        }
    }
```

이 함수는 게임 시작 시 초기 그리드를 생성합니다.

### 추가 행 생성하기

```csharp
   public void AddRow() // row생성 호출 시 추가 row 생성
    {
        gridHeight++;
        GameObject newRow = GenerateRow(new Vector3(0, -gridHeight + 1, 0));
        SpawnCharactersInRow(newRow.transform);
        currentRow = (currentRow == 1) ? 2 : 1;
    }
```

이 함수는 새로운 행을 그리드의 맨 위에 추가합니다.

### 행 생성 함수

```csharp
   private GameObject GenerateRow(Vector3 position)
    {
        GameObject rowPrefab = (currentRow == 1) ? row1Prefab : row2Prefab;
        return Instantiate(rowPrefab, position, Quaternion.identity, transform);
    }
```

이 함수는 주어진 위치에 새 행을 생성하고 반환합니다.

### 행에 캐릭터 배치하기

```csharp
private void SpawnCharactersInRow(Transform rowTransform)
    {
        for (int i = 0; i < rowTransform.childCount; i++)
        {
            Transform gridTransform = rowTransform.GetChild(i);
            GameObject characterPrefab = 
            characterManager.GetRandomCharacterPrefab();
            Vector3 newPosition = new Vector3(gridTransform.position.x, \
            gridTransform.position.y, -0.5f);
            GameObject characterInstance = 
            characterPool.GetPooledObject(characterPrefab);
            characterInstance.transform.position = newPosition;
            characterInstance.transform.SetParent(gridTransform, true);
            characterInstance.SetActive(true);

            // Disable all child objects except for the one named "level0"
            for (int j = 0; j < characterInstance.transform.childCount; j++)
            {
                Transform child = characterInstance.transform.GetChild(j);
                if (child.name != "level0")
                {
                    child.gameObject.SetActive(false);
                }
            }
        }
	}
```

이 함수는 주어진 행에 랜덤 캐릭터를 배치합니다.
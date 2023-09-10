---
title: 3 Match ReSpawn
category: unity
author: 이정훈
tags:
  - unity
  - c#
  - game
img: https://i.imgur.com/MdX9wST.gif
comments_disable: true
meta_description: 3 Match ReSpawn
---
# 3 Match Merge 이후 Respawn 로직

![](https://i.imgur.com/MdX9wST.gif)


```csharp
using System.Collections.Generic;
using UnityEngine;

public class SpawnManager : MonoBehaviour
{
    [SerializeField] private GridManager gridManager;
    [SerializeField] private CharacterPool characterPool;

    public void SpawnCharacters()
    {
        List<Vector2Int> availablePositions = new List<Vector2Int>();

        for (int x = 0; x < gridManager._gridWidth; x++)
        {
            for (int y = 0; y < gridManager._gridWidth; y++)
            {
                availablePositions.Add(new Vector2Int(x, y));
            }
        }
        int totalGridPositions = 
        gridManager._gridWidth * gridManager._gridHeight;
        int charactersToSpawn = totalGridPositions;
        for (int i = 0; i < charactersToSpawn; i++)
        {
            if (availablePositions.Count == 0)
            {
                Debug.LogWarning("Not enough available positions on the grid.");
                return;
            }

            int randomPositionIndex = Random.Range(0, availablePositions.Count);
            Vector2Int randomPosition = availablePositions[randomPositionIndex];
            availablePositions.RemoveAt(randomPositionIndex);

            SpawnCharacterAtPosition(randomPosition.x, randomPosition.y);
        }
    }
    public void SpawnCharacterAtPosition(int x, int y)
    {
        Vector3 spawnPosition = new Vector3(x, y, 0);
        if (!IsCharacterAtPosition(spawnPosition))
        {
            GameObject pooledCharacter = characterPool.GetPooledCharacter();

            if (pooledCharacter != null)
            {
                pooledCharacter.transform.position = spawnPosition;
                pooledCharacter.SetActive(true);
                gridManager.IncrementActiveGridCount();
            }
        }
    }
    public void RespawnCharacters()
    {
        int activeCharacterCount = characterPool.GetActiveCharacterCount();
        int activeGridCount = gridManager.GetActiveGridCount();
        if (activeCharacterCount < activeGridCount)
        {
            int _gridGap = activeGridCount - activeCharacterCount;

            for (int i = 0; i < _gridGap; i++)
            {

                GameObject characterToRespawn = 
                characterPool.GetRandomInactiveCharacter();
                GameObject gridToFill = gridManager.GetEmptyGrid();
                Debug.Log($"gridToFill: {gridToFill}");

                if (characterToRespawn != null && gridToFill != null)
                {
                    characterToRespawn.transform.position = 
                    gridToFill.transform.position;
                    characterToRespawn.SetActive(true);
                    gridToFill.SetActive(true);
                }
            }
        }
    }
    public bool IsCharacterAtPosition(Vector3 position)
    {
        foreach (GameObject character in characterPool.GetPooledCharacters())
        {
            if (character.activeInHierarchy 
            && character.transform.position == position)
            {
                return true;
            }
        }
        return false;
    }
    public GameObject GetCharacterAtPosition(Vector3 position)
    {
        foreach (GameObject character in characterPool.GetPooledCharacters())
        {
            if (character.activeInHierarchy 
            && character.transform.position == position)
            {
                return character;
            }
        }
        return null;
    }
    public List<GameObject> GetPooledCharacters()
    {
        return characterPool.GetPooledCharacters();
    }
}
```

Swipe 이후에 Grid에 비어있는 자리가 확인되면, 케릭터를 다시 스폰하고, 공간을 채우는 로직

하지만 Swipe 머지 이외의 자동으로 머지되는 상황을 인식하지 못함
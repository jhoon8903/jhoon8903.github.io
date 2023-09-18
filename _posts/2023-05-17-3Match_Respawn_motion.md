---
title: 3 Match_Respawn_motion
category: unity
author: 이정훈
tags:
  - unity
  - game
img: https://i.imgur.com/fcGrcTI.gif
comments_disable: true
meta_description: 3Match_Respawn_motion
---

![](https://i.imgur.com/fcGrcTI.gif)


# 3 Match Lerp를 이용한 Respawn 움직임

```csharp

using System.Collections.Generic;
using System.Linq;
using Script.CharacterManagerScript;
using UnityEngine;
using DG.Tweening;
using UnityEngine.TextCore.Text;
using System.Collections;

namespace Script
{
    public class SpawnManager : MonoBehaviour
    {
        [SerializeField]
        private GridManager gridManager;
        [SerializeField]
        private CharacterPool characterPool;
        
        // CharacterPool에서 사용 가능한 Pool 객체를 반환
        public List<GameObject> GetPooledCharacters()
        {
            return characterPool.GetPooledCharacters();
        }

        // Grid 전체에 케릭터 Object를 생성하는 메소드
        public void SpawnCharacters()
        {
            var availablePositions = new List<Vector2Int>();
            for (var x = 0; x < gridManager.gridWidth; x++)
            {
                for (var y = 0; y < gridManager.gridWidth; y++)
                {
                    availablePositions.Add(new Vector2Int(x, y));
                }
            }
            var totalGridPositions = 
            gridManager.gridWidth * gridManager.gridHeight;
            for (var i = 0; i < totalGridPositions; i++)
            {
                var randomPositionIndex = 
                Random.Range(0, availablePositions.Count);
                var randomPosition = availablePositions[randomPositionIndex];
                availablePositions.RemoveAt(randomPositionIndex);
                SpawnCharacterAtPosition(randomPosition.x, randomPosition.y);
            }
        }

        // 특정 Grid 좌표에 케릭터를 생성하는 메소드
        public GameObject SpawnCharacterAtPosition(int x, int y)
        {
            var spawnPosition = new Vector2(x, y);

            if (IsCharacterAtPosition(spawnPosition)) return null;
            var pooledCharacter = characterPool.GetPooledCharacter();

            if (pooledCharacter == null) return null;
            pooledCharacter.transform.position = spawnPosition;
            pooledCharacter.SetActive(true);

            return pooledCharacter;
        }

        // 특정 위치에 Character가 존재하는지 확인하는 메소드
        public bool IsCharacterAtPosition(Vector3 position)
        {
            return GetCharacterAtPosition(position) != null;
        }

        // 특정 위치에 있는 케릭터를 반환하는 메소드
        public GameObject GetCharacterAtPosition(Vector3 position)
        {
            var list = characterPool.GetPooledCharacters();
            return list.FirstOrDefault(character => character.activeInHierarchy 
            && character.transform.position == position);
        }

        // 비어있는 Grid 위에 Character를 이동 시키는 메소드
        public IEnumerator MoveCharactersEmptyGrid(Vector2 emptyGridPosition)
        {
            var tween = transform.DOMove(transform.position, 0);
            foreach (var character in characterPool.GetPooledCharacters())
            {
                if (character.transform.position.x != emptyGridPosition.x 
                ||!(character.transform.position.y < emptyGridPosition.y)) 
                continue;
                Vector2 newPosition = character.transform.position;
                newPosition.y += 1;
                tween = character.transform.DOMove(newPosition, 0.2f);
            }
            yield return tween.WaitForCompletion();
            RespawnCharacter();
        }

        // Pool에 활성화 되지 않은 CharacterObject를 확인하고 호출하는 메소드
        private void RespawnCharacter()
        {
            var inactiveCharacters = characterPool.GetPooledCharacters()
            .Where(character => !character.activeInHierarchy)
            .ToList();
            var freeXPositions = gridManager.GetFreeXPositions();
            var index = 0;
            for (; index < freeXPositions.Count; index++)
            {
                var t = freeXPositions[index];
                var randomCharacterIndex = 
                Random.Range((float)0, inactiveCharacters.Count);
                var character = inactiveCharacters[(int)randomCharacterIndex];
                character.transform.position = new Vector3(t, 0, 0);
                character.SetActive(true);
                inactiveCharacters.RemoveAt((int)randomCharacterIndex);
            }
        }
    }
}
```

Spawn 된 유닛과 Swipe 된 유닛이 겹치는 문제와 끝까지 채워지지 않는 문제 발생
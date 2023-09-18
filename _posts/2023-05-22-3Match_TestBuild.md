---
title: 3 Match_Test Build
category: Unity
author: 이정훈
tags:
  - unity
  - game
img: https://i.imgur.com/3gbSK0f.gif
comments_disable: true
meta_description: 3 Match_Test Build
---

![](https://i.imgur.com/3gbSK0f.gif)


3 Match 움직임 핵심 로직

```csharp
public IEnumerator PositionUpCharacterObject()
          {
              var moves = new List<(GameObject, Vector3Int)>();
              for (var x = 0; x < gridManager.gridWidth; x++) 
              { 
                  var emptyCellCount = 0; 
                  for (var y = gridManager.gridHeight - 1; y >= 0; y--) 
                  { 
                      var currentPosition = new Vector3Int(x, y, 0);
                      var currentObject = CharacterObject(currentPosition);
                      if (currentObject == null)
                      {
                          emptyCellCount++;
                      }
                      else if (emptyCellCount > 0)
                      {
                          var targetPosition = 
                          new Vector3Int(x, y + emptyCellCount, 0);
                          moves.Add((currentObject, targetPosition));
                      }
                  }
              }
              // PerformMoves를 시작하고 PositionUpCharacterObject 
              // 코루틴을 일시 중지하고 PerformMoves가 완료될 때까지 기다립니다.
              yield return StartCoroutine(PerformMoves(moves));
              // PerformMoves가 완료된 후 PositionUpCharacterObject가 
              // 특정 지연을 기다리며 계속됩니다.
              // SpawnAndMoveNewCharacters를 시작하고 PositionUpCharacterObject 
              // 코루틴을 일시 중지하고
              // SpawnAndMoveNewCharacters가 완료될 때까지 기다립니다.
              yield return StartCoroutine(SpawnAndMoveNewCharacters());
              // SpawnAndMoveNewCharacters가 완료된 후 PositionUpCharacterObject가 
              // 특정 지연을 기다리며 계속됩니다. 
              // CheckMatchesAndMoveCharacters를 시작하고 
              // PositionUpCharacterObject 코루틴을 일시 중지하고
              // CheckMatchesAndMoveCharacters가 완료될 때까지 기다립니다.
              yield return 
              StartCoroutine(matchManager.CheckMatchesAndMoveCharacters());
          }
```


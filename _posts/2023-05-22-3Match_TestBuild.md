---
title: 3 Match_Test Build
category: unity
author: 이정훈
tags:
  - unity
  - c#
  - game
img: Screen Recording 2023-09-11 at 3.22.27 AM.gif
comments_disable: true
meta_description: 3 Match_Test Build
---
![[Screen Recording 2023-09-11 at 3.22.27 AM.gif]]
<!--Upload failed, remote server returned an error: File is over the size limit-->


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


---
title: 3 Match_Merge
category: Unity
author: 이정훈
tags:
  - unity
  - game
img: https://i.imgur.com/87uT3wN.gif
comments_disable: true
meta_description: 3 Match_Merge
---

![](https://i.imgur.com/87uT3wN.gif)


3 4 5 및 특수매치 
각 매치 되는 배열을 인덱스로 분류하여 각 상황별 매치 업그레이드

```csharp
using System.Collections;
using System.Collections.Generic;
using Script;
using Script.CharacterManagerScript;
using UnityEngine;

public sealed class MatchManager : MonoBehaviour
{
    [SerializeField] private SpawnManager _spawnManager;

    private IEnumerator ReturnAndMoveCharacter(GameObject character)
    {
        yield return StartCoroutine(CharacterPool.ReturnToPool(character));
        yield return StartCoroutine(_spawnManager.
        MoveCharactersEmptyGrid(character.transform.position));
    }

    public bool IsMatched
    (GameObject swapCharacter, Vector3 swipeCharacterPosition)
    {
        var swapCharacterName = 
        swapCharacter.GetComponent<CharacterBase>()._characterName;
        var directions = new[]
        {
            (Vector3Int.left, Vector3Int.right, "Horizontal"), // Horizontal
            (Vector3Int.down, Vector3Int.up, "Vertical") // Vertical
        };

        var isMatchFound = false;
        var horizontalMatchCount = 0;
        var verticalMatchCount = 0;
        var matchedCharacters = new List<GameObject>();

        foreach (var (dir1, dir2, dirName) in directions)
        {
            var matchCount = 1; // To count the center character itself.
            var matchedObjects = new List<GameObject> { swapCharacter };

            foreach (var dir in new[] { dir1, dir2 })
            {
                var nextPosition = swipeCharacterPosition + dir;

                for (var i = 0; i < 2; i++)
                {
                    var nextCharacter = 
                    _spawnManager.GetCharacterAtPosition(nextPosition);
                    if (nextCharacter == null 
                    ||nextCharacter.GetComponent<CharacterBase>()._characterName 
                    != swapCharacterName)
                        break;
                    
                    matchedObjects.Add(nextCharacter);
                    matchCount++;
                    nextPosition += dir;
                }
            }
            
            if (dirName == "Horizontal")
                horizontalMatchCount += matchCount;
            else
                verticalMatchCount += matchCount;
            switch (matchCount)
            {
                case 1:
                case 2:
                case 3:
                case 4:
                case 5:
                    break;
            }
            matchedCharacters.AddRange(matchedObjects);
        }

        if (horizontalMatchCount + verticalMatchCount == 4)
        {
            switch (horizontalMatchCount)
            {
                case 1:
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                    isMatchFound = true;
                    return isMatchFound;
                case 3:
                    if (swipeCharacterPosition.x 
                    == matchedCharacters[1].transform.position.x)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }
                    
                    if (swipeCharacterPosition.x ==
                    matchedCharacters[2].transform.position.x)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[1]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }        
                    if (swipeCharacterPosition.x == 
                    matchedCharacters[3].transform.position.x)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[1]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    matchedCharacters[3].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }

                    return isMatchFound;
            }
        }

        if (horizontalMatchCount + verticalMatchCount == 5)
        {
            switch (horizontalMatchCount)
            {
                case 1:
                    if (swipeCharacterPosition.y > 
                    matchedCharacters[2].transform.position.y && 
                    swipeCharacterPosition.y < 
                    matchedCharacters[3].transform.position.y)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[4]));
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[3].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }
                    if (swipeCharacterPosition.y > 
                    matchedCharacters[3].transform.position.y &&
                    swipeCharacterPosition.y < 
                    matchedCharacters[4].transform.position.y)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[4]));
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }

                    return isMatchFound;

                case 2:
                    if (swipeCharacterPosition == 
                    matchedCharacters[2].transform.position)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[4]));
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }
                    return isMatchFound;

                case 3:
                    if (swipeCharacterPosition == 
                    matchedCharacters[3].transform.position)
                    {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[1]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    matchedCharacters[3].GetComponent<CharacterBase>().LevelUp();
                    isMatchFound = true;
                        return isMatchFound;
                    }

                    return isMatchFound;

                case 4:
                    if (swipeCharacterPosition == 
                    matchedCharacters[4].transform.position && 
                    matchedCharacters[1].transform.position.x < 
                    swipeCharacterPosition.x && 
                    matchedCharacters[2].transform.position.x > 
                    swipeCharacterPosition.x)
                    {
                        Debug.Log("2번 스왑");
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[1]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[4].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }

                    if (swipeCharacterPosition == 
                    matchedCharacters[4].transform.position &&
                    matchedCharacters[1].transform.position.x < 
                    swipeCharacterPosition.x &&
                    matchedCharacters[3].transform.position.x > 
                    swipeCharacterPosition.x)
                    {
                        Debug.Log("3번 스왑");
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[4].GetComponent<CharacterBase>().LevelUp();
                        isMatchFound = true;
                        return isMatchFound;
                    }

                    return isMatchFound;
            }
        }

        if (horizontalMatchCount + verticalMatchCount == 6)
        {
            switch (horizontalMatchCount)
            {
                case 1:
                    if (swipeCharacterPosition != 
                    matchedCharacters[1].transform.position) return isMatchFound;
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[5]));
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[4].GetComponent<CharacterBase>().LevelUp();
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[2]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[4]));
                    matchedCharacters[1].GetComponent<CharacterBase>().LevelUp();
                    isMatchFound = true;
                    return isMatchFound;

                case 2:

                    if (swipeCharacterPosition.y > 
                    matchedCharacters[3].transform.position.y && 
                    swipeCharacterPosition.y < 
                    matchedCharacters[5].transform.position.y)
                    {
                        if (swipeCharacterPosition.y > 
                        matchedCharacters[4].transform.position.y)
                        {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[5]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[4]));
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[3].GetComponent<CharacterBase>().LevelUp();
                            isMatchFound = true;
                            return isMatchFound;
                        }

                        if (swipeCharacterPosition.y <
                        matchedCharacters[4].transform.position.y)
                        {
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[5]));
                    StartCoroutine(ReturnAndMoveCharacter(matchedCharacters[3]));
                    matchedCharacters[2].GetComponent<CharacterBase>().LevelUp();
                    matchedCharacters[4].GetComponent<CharacterBase>().LevelUp();
                            isMatchFound = true;
                            return isMatchFound;
                        }
                    }

                    return isMatchFound;

                case 3:...
}
```

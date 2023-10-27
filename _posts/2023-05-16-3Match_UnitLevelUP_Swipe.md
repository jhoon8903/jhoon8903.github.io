---
title: 3 Match Char LevelUp && Swipe
category: Unity
author: 이정훈
tags:
  - unity
  - game
img: https://i.imgur.com/TbuHkNr.gif
comments_disable: true
meta_description: 3 Match Char LevelUp && Swipe
---

![](https://i.imgur.com/TbuHkNr.gif)


# 3 Match Merge Character LevelUp 및 Lerp를 이용한 Swipe 움직임

```csharp
using System.Collections;
using Script.CharacterManagerScript;
using UnityEditor.UIElements;
using UnityEngine;
using UnityEngine.Serialization;

namespace Script
{
    public class SwipeManager : MonoBehaviour
    {
        private GameObject _startObject;
        private GameObject _returnObject; 
        private Vector2 _initialScale; 
        private Vector2 _firstTouchPosition; 
        private Vector2 _emptyGridPosition; 
        public float duration;
        [SerializeField] private float _minSwipeLength = 0.2f;
        [SerializeField] private SpawnManager spawnManager; 
        [SerializeField] private CountManager countManager; 
        [SerializeField] private LayerMask characterLayer; 
        [SerializeField] private MatchManager matchManager;
        [SerializeField] private GridManager gridManager;
        
        private void Update()   
        {
            if (Input.GetMouseButtonDown(0))
            {
                if (Camera.main != null)
                {
                    Vector2 worldPoint = 
                    Camera.main.ScreenToWorldPoint(Input.mousePosition);
                    var point2D = new Vector2(worldPoint.x, worldPoint.y);
                    var hit = Physics2D.Raycast
                    (point2D, Vector2.zero, 0f, characterLayer);
                    if (hit.collider != null)
                    {
                        if (_startObject != null)
                        {
                            _startObject.transform.localScale = _initialScale;
                        }
                        _startObject = hit.collider.gameObject;
                        _initialScale = _startObject.transform.localScale;
                        _startObject.transform.localScale = 
                        _initialScale * 1.2f; 
                        _firstTouchPosition = point2D;
                    }
                }
            }

            if (Input.GetMouseButtonUp(0))
            {
                HandleTouchEnd();
            }

            if (!Input.GetMouseButton(0)) return;
            {
                // if (_startObject == null) return;
                if (Camera.main == null) return;
                Vector2 worldPoint = 
                Camera.main.ScreenToWorldPoint(Input.mousePosition);
                var point2D = new Vector2(worldPoint.x, worldPoint.y);
                var swipe = point2D - _firstTouchPosition;
                if (!(swipe.sqrMagnitude > _minSwipeLength * _minSwipeLength)) 
                return;
                _firstTouchPosition = point2D;
                HandleSwipe(swipe);
            }
        }
    
        // CountManager의 Count를 참조하여 이동가능횟수를 확인
        private bool CanMove()  
        {
            return countManager.CanMove();
        }
    
        // 사용자 편의를 위해 스와이프 각도를 보정하여 스와이프 각도에 따라 처리합니다.
        private void HandleSwipe(Vector2 swipe)
        {
            if (!CanMove()) return;
            if (_startObject == null) return;

            var swipeAngle = Mathf.Atan2(swipe.y, swipe.x) * Mathf.Rad2Deg;
            swipeAngle = (swipeAngle < 0) ? swipeAngle + 360 : swipeAngle;

            var position = _startObject.transform.position;
            var startX = (int)position.x;
            var startY = (int)position.y;
            var endX = startX;
            var endY = startY;

            switch (swipeAngle)
            {
                case >= 315 or < 45:
                    endX += 1;
                    break;
                case >= 45 and < 135:
                    endY += 1;
                    break;
                case >= 135 and < 225:
                    endX -= 1;
                    break;
                case >= 225 and < 315:
                    endY -= 1;
                    break;
            }
            var endObject = 
            spawnManager.GetCharacterAtPosition(new Vector3(endX, endY, 0f));
            if (endObject != null)
            {
                HandleSwap(startX, startY, endX, endY, endObject);
            }
            else
            {
                StartCoroutine(NullSwap(startX, startY, endX, endY));
            }
            _startObject = null;
        }

        private IEnumerator AutoMatchAfterSwap()
        {
            while (true)
            {
                yield return new WaitForEndOfFrame();
                var isMatched = false;
                for (var x = 0; x < gridManager.gridWidth; x++)
                {
                    for (var y = 0; y < gridManager.gridHeight; y++)
                    {
                        if (matchManager.IsMatched(new Vector3(x, y, 0f)))
                        {
                            isMatched = true;
                        }
                    }
                }
                if (!isMatched) break;
            }
        }
        
        //  터치가 끝나면 초기화 합니다. (터치시 호버 작동 터치 완료 후 호버 초기화)
        private void HandleTouchEnd()
        {
            if (_startObject == null) return;
            _startObject.transform.localScale = _initialScale;
            _startObject = null;
        }

        //  케릭터 오브젝트가 빈 Grid로 스와이프되면 오브젝트를 Pool 반환 하는 기능
        private IEnumerator NullSwap(int startX, int startY, int endX, int endY)
        {
            Vector2 startPosition = _startObject.transform.position;
            Vector2 endPosition = new Vector3(endX, endY, 0f);
            _startObject.transform.position = endPosition;
            countManager.DecreaseMoveCount();
            _returnObject = _startObject;
            _startObject.transform.localScale = _initialScale;
            _emptyGridPosition = new Vector2(startX, startY);
            // Debug.Log(_emptyGridPosition);
        
            yield return new WaitForSeconds(0.1f);

            CharacterPool.ReturnToPool(_returnObject);
            spawnManager.MoveCharactersEmptyGrid(_emptyGridPosition);
        }
        
        private void HandleSwap
        (int startX, int startY, int endX, int endY, GameObject endObject)
        {
            if (_startObject == null) return;
            var startPosition = new Vector3(startX, startY, 0);
            var endPosition = new Vector3(endX, endY, 0f);
            StartCoroutine(SwapAndMatch
            (_startObject, endObject, endPosition, startPosition));
            _startObject.transform.localScale = _initialScale;
            countManager.DecreaseMoveCount();
        }
        private IEnumerator SwapAndMatch(GameObject startObject, GameObject
         endObject, Vector3 endPosition, Vector3 startPosition)
        {
            yield return MoveOverTime(startObject, endPosition);
            yield return MoveOverTime(endObject, startPosition);
            matchManager.IsMatched(endPosition);
            StartCoroutine(AutoMatchAfterSwap());
        }
        private IEnumerator MoveOverTime(
        GameObject objectToMove, Vector3 destination)
        {
            duration = 0.2f;
            float elapsedTime = 0;
            var startingPos = objectToMove.transform.position;
            while (elapsedTime < duration)
            {
                objectToMove.transform.position = 
                Vector3.Lerp(startingPos, destination, (elapsedTime / duration));
                elapsedTime += Time.deltaTime;
                yield return null;
            }
            objectToMove.transform.position = destination;
            yield return new WaitUntil(() => 
            objectToMove.transform.position == destination);
        }
    }
}
```


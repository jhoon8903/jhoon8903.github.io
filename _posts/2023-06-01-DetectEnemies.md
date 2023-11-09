---
layout: post
title: DetectEnemies
subtitle: 적 감지와 공격
categories: Project
author: Daniel
tags:
 - Unity
 - Game
 - Physic
 - Object
 - Script
banner:
 image: https://i.imgur.com/0u72cd4.gif
---

|Gizmo|Action|
|:--:|:--:|
|![300](https://i.imgur.com/pWJd6gP.png)|![](https://i.imgur.com/0u72cd4.gif)|




## Vertical Area Detecte

```csharp
public override List<GameObject> DetectEnemies()
        {
            Debug.Log("DetectEnemies");
            var detectionSize = new Vector2(DetectionWidth, DetectionHeight);
            var detectionCenter = (Vector2)transform.position + Vector2.up * 
            DetectionHeight / 2f;
            var colliders = Physics2D.OverlapBoxAll(detectionCenter, 
            detectionSize, 0f);
            return (from collider in colliders
                    where collider.gameObject
                        .CompareTag("Enemy")
                    select collider.gameObject)
                .ToList();
        }

        public void OnDrawGizmosSelected()
        {
            Debug.Log("ONDrawGizmos");
            var detectionSize = new Vector3(DetectionWidth, DetectionHeight, 0);
            var detectionCenter = 
            (Vector3)transform.position * DetectionHeight / 2f;
            Gizmos.color = Color.yellow;
            Gizmos.DrawWireCube(detectionCenter, detectionSize);
        }
```

## Circle Area Detecte

```csharp
public override List<GameObject> DetectEnemies()
    {
        var detectionCenter = (Vector2)transform.position;
        var colliders = Physics2D.OverlapCircleAll(detectionCenter, 
        detectionSize);
        var detectedEnemies = (from collider in colliders
            where collider.gameObject.CompareTag("Enemy")
            select collider.gameObject).ToList();
        foreach (var enemy in detectedEnemies)
        {
            Debug.Log($"DetectEnemies_Unit_D: " +
                      $"Detected enemy {enemy.name} " +
                      $"at position {enemy.transform.position}");
        }
        return detectedEnemies;
    }

    public void OnDrawGizmos()
    {
        var detectionCenter = transform.position;
        Gizmos.color = Color.cyan;
        Gizmos.DrawWireSphere(detectionCenter, detectionSize);
    }
```

Physics2D.Overlap(Circle, Box)All 을 이용하여 해당 콜라이더 안으로 들어온 Target 콜라이더를 감지하여
적 오브젝트 및 Transform.position을 List로 반환하는 로직



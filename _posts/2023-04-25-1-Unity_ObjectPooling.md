---
title: 게임 개발 5일차 Unity ObjectPooling 트러블
category: Unity
author: 이정훈
tags:
  - unity
  - game
img: https://i.imgur.com/BijPhE7.png
comments_disable: true
meta_description: Unity ObjectPooling 트러블
---

## Object Polling 최적화

문제 : Polling된 Prefabs 가 Player Collider 충돌시 Destroy 되면서 
Pool 에서 삭제가 되 불러올 수 없는 상황


![](https://i.imgur.com/BijPhE7.png)

```c#
using System.Collections.Generic;
using UnityEngine;

public class ObstaclePool : MonoBehaviour
{
    public List<GameObject> obstaclePrefabs;
    public int pooledAmount = 10;
    public float obstacleLifetime = 5.0f;

    private List<GameObject> pooledObstacles;

    void Start()
    {
        pooledObstacles = new List<GameObject>();

        for (int i = 0; i < pooledAmount; i++)
        {
            int prefabIndex = Random.Range(0, obstaclePrefabs.Count);
            GameObject obstacle = Instantiate(obstaclePrefabs[prefabIndex]);
            obstacle.SetActive(false);
            obstacle.transform.SetParent(transform);
            pooledObstacles.Add(obstacle);
        }
    }

    public GameObject GetPooledObstacle(int prefabIndex)
    {
        for (int i = 0; i < pooledObstacles.Count; i++)
        {
            if (pooledObstacles[i] != null && !pooledObstacles[i].activeInHierarchy && pooledObstacles[i].name.Contains(obstaclePrefabs[prefabIndex].name))
            {
                return pooledObstacles[i];
            }
        }

        GameObject newObj = Instantiate(obstaclePrefabs[prefabIndex]);
        newObj.SetActive(false);
        pooledObstacles.Add(newObj);
        return newObj;
    }


    public void ReturnToPool(GameObject obstacle)
    {
        if (obstacle != null)
        {
            obstacle.SetActive(false);
        }
    }

}

```

```c#
using System.Collections;
using UnityEngine;

public class ObstacleSpawner : MonoBehaviour
{
    public ObstaclePool obstaclePool;
    public float spawnDelay;
    public float obstacleLifetime;
    public float spawnChance;

    private Transform playerBody;
    private float timeSinceLastSpawn = 0.0f;

    void Start()
    {
        playerBody = GameObject.FindGameObjectWithTag("PlayerBody").transform;
    }

    void Update()
    {
        if (playerBody == null) return;

        timeSinceLastSpawn += Time.deltaTime;

        if (timeSinceLastSpawn >= spawnDelay)
        {
            float spawnRoll = Random.value;

            if (spawnRoll < spawnChance)
            {
                Vector2 spawnPosition = new Vector2(playerBody.position.x + Random.Range(-2.0f, 2.0f), playerBody.position.y + Random.Range(-3.0f, 3.0f));

                // Check for overlapping obstacles
                float checkRadius = 1.0f; // Adjust this value to the appropriate size for your obstacles
                Collider2D[] overlaps = Physics2D.OverlapCircleAll(spawnPosition, checkRadius);

                bool isOverlap = false;
                foreach (Collider2D overlap in overlaps)
                {
                    if (overlap.gameObject.CompareTag("Obstacle"))
                    {
                        isOverlap = true;
                        break;
                    }
                }

                // Spawn obstacle only if there is no overlap
                if (!isOverlap)
                {
                    int prefabIndex = Random.Range(0, obstaclePool.obstaclePrefabs.Count);
                    GameObject obstacle = obstaclePool.GetPooledObstacle(prefabIndex);
                    obstacle.transform.position = spawnPosition;
                    obstacle.SetActive(true);
                    StartCoroutine(DeactivateAfterDelay(obstacle, obstacleLifetime));
                }
            }

            timeSinceLastSpawn = 0.0f;
        }
    }


    public IEnumerator DeactivateAfterDelay(GameObject obstacle, float delay)
    {
        yield return new WaitForSeconds(delay);
        obstaclePool.ReturnToPool(obstacle);
    }
}

```

Update Destory 부분 삭제 및 Pool 반환하도록 변경
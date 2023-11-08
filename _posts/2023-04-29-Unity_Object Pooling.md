---
layout: post
title: Unity Object Pooling & Respawn
subtitle: 오브젝트 풀링하여 장애물을 리스폰하는 기능
categories: Project
author: Daniel
tags: 
 - Unity
 - Game
 - Project
 - Object
 - Script
img: https://i.imgur.com/uV1fePt.gif
comments_disable: true
meta_description: Unity Object Pooling Respawn
---


![](https://i.imgur.com/uV1fePt.gif)


``` c#
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class ObstaclePool : MonoBehaviour
{
    public static ObstaclePool Instance { get; private set; }

    private List<GameObject> pooledObstacles;
    public List<GameObject> obstaclePrefabs;
    private int pooledAmount = 50;

    private void Awake()
    {
        if (Instance != null)
        {
            Destroy(gameObject);
            return;
        }

        Instance = this;
        DontDestroyOnLoad(gameObject);
    }

    private void OnEnable()
    {
        SceneManager.sceneLoaded += OnSceneLoaded;
    }

    void Start()
    {
        InitializePool();
    }

    private void OnDisable()
    {
        SceneManager.sceneLoaded -= OnSceneLoaded;
    }

    private void OnSceneLoaded(Scene scene, LoadSceneMode mode)
    {
        InitializePool();
    }

    private void InitializePool()
    {
        pooledObstacles = new List<GameObject>();

        for (int i = 0; i < pooledAmount; i++)
        {
            int prefabIndex = Random.Range(0, obstaclePrefabs.Count);
            GameObject obstacle = Instantiate(obstaclePrefabs[prefabIndex]);
            obstacle.name = obstaclePrefabs[prefabIndex].name;
            obstacle.SetActive(false);
            obstacle.transform.SetParent(transform);
            obstacle.transform.position = new Vector2(obstacle.transform.position.x, obstacle.transform.position.y);
            pooledObstacles.Add(obstacle);
        }
    }


    public GameObject GetPooledObstacle(int prefabIndex)
    {
        if (prefabIndex < 0 || prefabIndex >= obstaclePrefabs.Count)
        {
            return null;
        }

        for (int i = 0; i < pooledObstacles.Count; i++)
        {
            if (pooledObstacles[i] != null && !pooledObstacles[i].activeInHierarchy && pooledObstacles[i].name.Contains(obstaclePrefabs[prefabIndex].name))
            {
                return pooledObstacles[i];
            }
        }

        GameObject newObj = Instantiate(obstaclePrefabs[prefabIndex]);
        newObj.name = obstaclePrefabs[prefabIndex].name;
        newObj.SetActive(false);
        newObj.transform.position = new Vector2(newObj.transform.position.x, newObj.transform.position.y);
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
using System;
using System.Collections;
using UnityEngine;
using Random = UnityEngine.Random;

public abstract class ObstacleSpawner : MonoBehaviour
{
    protected ObstaclePool obstaclePool;
    protected float spawnDelay;
    protected float obstacleLifetime;
    protected float spawnChance;

    public event Action<GameObject> OnObstacleDeactivated;

    protected Transform playerBody;
    protected float timeSinceLastSpawn = 5.0f;


    protected virtual void Start()
    {
        playerBody = GameObject.FindGameObjectWithTag("PlayerBody").transform;
    }

    void Update()
    {
        if (playerBody == null || obstaclePool == null) return;

        timeSinceLastSpawn += Time.deltaTime;

        if (timeSinceLastSpawn >= spawnDelay)
        {
            SpawnObstacle();
            timeSinceLastSpawn = 0.0f;
        }
    }


    protected virtual void SpawnObstacle()
    {
        float spawnRoll = Random.value;



        if (spawnRoll < spawnChance)
        {
            Vector2 spawnPosition = GetSpawnPosition();
            float checkRadius = GetCheckRadius();
            Collider[] overlaps = Physics.OverlapSphere(spawnPosition, checkRadius, LayerMask.GetMask("Default"));

            bool isOverlap = false;
            foreach (Collider overlap in overlaps)
            {
                if (overlap.gameObject.CompareTag("Obstacle"))
                {
                    isOverlap = true;
                    break;
                }
            }
            if (!isOverlap)
            {
                int prefabIndex = Random.Range(0, obstaclePool.obstaclePrefabs.Count);
                GameObject obstacle = obstaclePool.GetPooledObstacle(prefabIndex);
                obstacle.transform.position = spawnPosition;
                obstacle.SetActive(true);
                StartCoroutine(DeactivateAfterDelay(obstacle, obstacleLifetime));
            }
        }
    }

    protected virtual Vector2 GetSpawnPosition()
    {
        return new Vector2(Random.Range(-4.0f, 4.0f), playerBody.position.y + Random.Range(2.0f, 7.0f));
    }

    protected virtual float GetCheckRadius()
    {
        return 0.8f;
    }

    protected virtual IEnumerator DeactivateAfterDelay(GameObject obstacle, float delay)
    {
        yield return new WaitForSeconds(delay);

        OnObstacleDeactivated?.Invoke(obstacle);
        obstaclePool.ReturnToPool(obstacle);
    }

}

```
---
title: Unity ObjectPooling 트러블
category: unity
author: "이정훈"
tags: [unity, c#, game]
img : https://i.imgur.com/BijPhE7.png
comments_disable: true
meta_description: "Unity ObjectPooling 트러블"
---

## Object Polling 최적화

문제 : Polling된 Prefabs 가 Player Collider 충돌시 Destroy 되면서 
Pool 에서 삭제가 되 불러올 수 없는 상황


![](https://i.imgur.com/BijPhE7.png)


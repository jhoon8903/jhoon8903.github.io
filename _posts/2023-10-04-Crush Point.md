---
layout: post
title: Unity 충돌 지점과 법선 벡터
subtitle: Collider 충돌과 벡터
categories: Unity
author: Daniel
tags: 
 - Unity
 - Physic
 - Vector
banner:
 image: https://i.imgur.com/hMPvXdL.jpg
---
Normal Vector (법선 벡터)
--

- 두 물체가 충돌한 평면 또는 구면의 접점에서 수직 방향으로 바라보는 벡터
![](https://i.imgur.com/hMPvXdL.jpg)

## 충돌지점 정보 

```csharp
Collision Class

public ContactPoint GetContact(int index); // 단일 충돌 정보
public int GetContact(ContactPoint[] contacts); // 다중 충돌 정보

struct ContactPoint 
{
	normal // 충돌 지점의 법선
	otherCollider // 충돌 지점의 다른 Collider
	point // 충돌 지점의 위치
	separation // 충돌한 두 Collider 간의 거리
	thisCollider // 충돌 지점의 첫 번째 Collider
}

```

```csharp
// 첫 번째 충돌 지점의 정보 추출
ContactPoint cp = coll.GetContact(0);
cp.point // coll의 첫 번째 충돌 지점의 위치
cp.normal // coll의 첫 번째 충돌 지점의 법선
```
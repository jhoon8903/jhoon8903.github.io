---
title: Unity {get; set;} 자동구현 Property
category: unity
author: "이정훈"
tags: [unity, c#]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity {get; set;} 자동구현 Property"
---

```c#
public class CubeSpawner : MonoBehaviour
{
	public Transform LastCube { set; get; }
	public MovingCube CurrentCube { set; get; } = null;
}
```
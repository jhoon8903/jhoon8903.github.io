---
layout: post
title: Unity Vector3 회전
subtitle: Rotate를 이용한 오브젝트 회전
categories: Unity
author: Daniel
tags:
 - Unity
 - Vector
 - Transform
banner:
 image: https://i.imgur.com/bBfMxFd.gif
---


Rotate (회전)
--

![](https://i.imgur.com/bBfMxFd.gif)


기초적인 Rotate 함수
- void Rotate (Vector3 eulerAngles, [Space relativeTo]);
- void Rotate (float xAngle, float yAngle, float zAngle, [Spave relativeTo]);
- void Rotate (Vector3 axis, float angle, [Space relativeTo]);

🌝 y축을 기준으로 30도 회전시키는 예제
```csharp
tr.Rotate(new Vector3(0.0f, 30.0f, 0.0f));
tr.Rotate(0.0f, 30.0f, 0.0f);
tr.Rotate(Vector3.up * 30.0f);
```

⭐️ 마우스의 움직으로 회전을 하는 방법
```csharp
var rotate = Input.GetAxis("Mouse X");

_playerTr.Rotate(Vector3.up * (turnSpeed * Time.deltaTime * rotate));
```

-  마우스의 경우 왼쪽으로 움직이면 **"-"** , 오른쪽으로 움직이면 **"+"** 반환
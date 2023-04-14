---
title: Unity_GetButton
category: unity
author: "이정훈"
tags: [unity, c#]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity_GetButton"
---

# Input.GetButton, GetButtonDown, GetButtonUp

## Input.GetButton
- 버튼을 누르면 True 이벤트 발생

## Input.GetButtonDown
- 버튼을 누를 때 한번 True가 발생

## Input.GetButtonUP
- 버튼을 눌렀다가 땠을 경우 True가 발생

Edit > ProjectSetting > Input의 Axes에 있는 입력 키를 사용

![](https://i.imgur.com/oLBvUcx.png)

```c#
if (Input.GetButtonDown("fire1"));
```


![](https://i.imgur.com/Y76dIob.png)

---
title: Bonn 최적화
category: Unity
author: 이정훈
tags: [Unity, Modeling]
img: https://i.imgur.com/e3TNb4M.jpg
comments_disable: true
meta_description: Bonn 최적화
---

# Bone Structure Optimization

- 기존의 Bone 구조 
- 각 관절(Joint)는 GameObject이며, Transform 컴포넌트를 가지고 있음
- 많은 관절을 가진 3D 모델의 경우 Transform 컴포넌트의 Position, Rotation 처리는 런타임 시 내부적으로 다양한 연산을 처리하므로 최적화 (Optimization)이 필요함

### ❗️ 최적화 시점
- Bone 구조의 최적화 하는 Optimize game Object 옵션의 경우 반드시 **프리팹으로 전환하기 전에** 해야함

![](https://i.imgur.com/kYOb4tk.jpg)

![](https://i.imgur.com/GSt3poi.jpg)

![](https://i.imgur.com/D00DyGE.jpg)

## 1. 3D Model Object Select 
- 3D 모델의 게임오브젝트를 선택 후 Inspector에 Rig 선택
- 
![](https://i.imgur.com/AwfHRg2.jpg)

## 2. Optimize Game Objects 
- Optimize Game Objects  => true
![](https://i.imgur.com/XLYtDOu.jpg)

![](https://i.imgur.com/e3TNb4M.jpg)

- 하이어라키에 노출 시킬 Rig를 선택 => Apply

![](https://i.imgur.com/wNZu3Bs.jpg)

- 하이어 라키에 표시되는 Bone 
![](https://i.imgur.com/RGLWQFM.jpg)

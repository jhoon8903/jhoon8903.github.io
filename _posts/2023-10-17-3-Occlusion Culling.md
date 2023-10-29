---
title: Unity 오클루전 컬링
category: Unity
author: 이정훈
tags: [Unity, Light]
img: https://i.imgur.com/LGfzqsQ.gif
comments_disable: true
meta_description: Unity Input System
---
# 오클루전 컬링 (Occlusion Culling)
- 렌더링 부하를 줄여주는 기법 중 하나
- 3D 게임 및 콘텐츠 개발에 필수적 요소
##### 오클루전 (Occlusion) : 한 물체가 다른 물체를 가리는 것
##### 컬링 (Culling) : 도태시킨다 (제거한다)
❗️Occlusion Culling 는 가려진 물체는 제거한다라는 의미
- 카메라에 보이지 않는 객체는 렌더링하지 않는다
- 불필요한 요소를 렌더링에서 제외함으로써 렌더링 부하를 줄이고 속도를 향상시킴

## 컬링 방식
### 프러스텀 컬링 (Frustum Culling)
- 카메라의 시야에 범위에 들어와 있는 물체만을 렌더링 하고 시야 범위 밖의 물체는 렌더링 하지 않음
![](https://i.imgur.com/6Kfbvt6.jpg)


![](https://i.imgur.com/jNr3mFH.jpg)

![](https://i.imgur.com/b7Zs4VC.jpg)

![](https://i.imgur.com/q2055fr.jpg)
⭕️ Main Camera의 프리스텀 영역과 Clipping Planes

### 거리 비례에 의한 컬링 (LOD Culling)
- 카메라로부터 너무 멀리 떨어져 있는 Object에 대해서 [[2023-09-28-Level of Detail|LOD]] 적용
![](https://i.imgur.com/xSr2nnO.jpg)

### Occlusion Culling
- 카메라 시야에서 다른 물체에 가려 보이지 않는 물체를 렌더링 하지 않는 기법
![](https://i.imgur.com/bHv5Irg.jpg)

- Occluder Static Flag 설정
	- 다른 오브젝트를 가리는 객체는 => Occluder Static
	- 가려지는 오브젝트 => Occludee Static

![](https://i.imgur.com/AWE4LEG.jpg)

![](https://i.imgur.com/1Q9IvpL.jpg)

![](https://i.imgur.com/LGfzqsQ.gif)

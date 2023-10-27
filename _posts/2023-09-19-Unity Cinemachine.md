---
title: Unity Cinemachine
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/JvHL268.png
comments_disable: true
meta_description: Unity Cinemachine
---

# Cinemachine

- 카메라 움직임을 손쉽게 제어하는 Unity 공식 Package
- 카메라 연출에 필요한 코드와 조정 작업 대부분을 대체
- 초점, 피사체 배치, 추적 지연시간, 흔들림, 전환 등을 컴포넌트로 제공

## Cinemachine의 원리

### 브레인 카메라 (Brain Camera)
- 게임월드를 촬용하는 '진짜' 카메라 Scene에 '1개' 만 존재함
### 가상 카메라 (Virtual Camera)
- 브레인카메라의 '분신'역할 Scene에 여러개 존재할 수 있음


![](https://i.imgur.com/JvHL268.png)


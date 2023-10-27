---
title: Unity Anchor Preset
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/HzkrUoY.jpg
comments_disable: true
meta_description: Unity Anchor Preset
---

# Anchor Preset
![](https://i.imgur.com/ufpsFs3.jpg)
Mac 기준 Option (⌥), Shift 조합에 따라 다르게 표현됨

|기본 Preset|Option (⌥)|
|:--:|:--:|
|![](https://i.imgur.com/HzkrUoY.jpg)|![](https://i.imgur.com/jweYk0a.jpg)|
|Shift|Option (⌥) + Shift|
|![](https://i.imgur.com/CzDNQEg.jpg)|![](https://i.imgur.com/9NgabWw.jpg)|


## 기본 Preset
### 정중앙 정렬 (middle / center)
- Resolution 이 변경 되어도 해당 UI가 화면 가운데 정렬되는 Anchor Preset

![](https://i.imgur.com/Q8ggCAA.jpg)

### 왼쪽 상단 정렬 (top / left)
- Resolution 이 변경 되어도 왼쪽 상단에 정렬되는 Anchor Preset
- UI Size는 변경 되지 않음

![](https://i.imgur.com/a5CUq2M.jpg)

### 왼쪽 상하 리사이즈 정렬 (stretch / left)
- 왼쪽으로 정렬 된 상태로 세로 길이가 Resolution 에 맞게 늘어난다.

![](https://i.imgur.com/fQn5zla.jpg)

## Option ⌥ Preset
- 선택 된 UI 항목을 해당 위치로 이동 및 정렬

![](https://i.imgur.com/ETuCSa1.jpg)

## Shift Preset
- Pivot 위치만 변경하며, 위치 이동은 하지 않음

![](https://i.imgur.com/azp0D4J.jpg)

## Option ⌥ + Shift
- Pivot 위치를 일치키시며, 해당 포인트로 UI 항목이 이동함

![](https://i.imgur.com/3QsfMLC.jpg)

## Anchor 속성

- 앵커는 네 개의 작은 화살표로 표시
![](https://i.imgur.com/XsjlQwq.jpg)

- 네 개의 앵커 설정 값의 경우 0.0f ~ 1.0f (0.5f 는 50%를 의미)
![](https://i.imgur.com/haMsq9H.jpg) 

![](https://i.imgur.com/fwTtDyN.jpg)

- Min (0.0f, 0.0f) / Max (1.0f, 1.0f)
- Pivot (0.5f, 0.5f)
![](https://i.imgur.com/1cKapMM.jpg)

- Min (0.2f, 0.2f) / Max (0.8f, 0.8f)
- Pivot (0.5f, 0.5f)
![](https://i.imgur.com/CL5NEVA.jpg)
![](https://i.imgur.com/C1J1eqM.jpg)


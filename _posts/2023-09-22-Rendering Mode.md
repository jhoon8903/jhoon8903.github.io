---
layout: post
title: Unity Rendering Mode
subtitle: 오브젝트 랜더링 모드에 대해서
categories: Unity
author: Daniel
tags: 
 - Unity
 - Rendering
banner:
 image: https://i.imgur.com/WFyLaBe.gif
---

Rendering Mode
==

![](https://i.imgur.com/l6LMLai.png)

|Rendering Mode Option|Notice|
|:--:|:--|
|Opaque (불투명)|기본값으로 불투명한 텍스처를 표현하는 옵션<br>투명한 부분이 전혀 없는 Soild 객체에 적합|
|Cutout (그물망 표현)|불투명한 부분과 투명한 부분을 동시에 표현하는 옵션<br>주로 풀, 그물망 등을 표현할때 적합|
|Fade (홀로그램 표현)|투명 속성값을 가진 객체를 페이드 아웃시키는 옵션<br>페이트인/아웃을 애니메이션처럼 처리할 수 있음<br>불투명한 객체를 부분적으로 페이드 아웃시킬 수 있어 홀로그램(Hologram)효과를 구현할 수 있음|
|Transparent (투명)|투명한 플라스틱 또는 유리와 같은 재질을 표현하는 옵션|

- Opaque
![](https://i.imgur.com/92Dh5Iq.jpg)

- Cutout
![](https://i.imgur.com/Pl452Iw.png)

- Fade
![](https://i.imgur.com/XNKft8u.jpg)

- Transparent
![](https://i.imgur.com/lsGCbZP.jpg)


Albedo (알베도)
--

- 빛을 반사하는 정도 (반사율)
- 가장 기본이 되는 텍스터를 연결하는 속성
![](https://i.imgur.com/T3kIYMy.png)

Metallic (메탈릭)
--

- 금속의 재질을 표현하기 위한 텍스처
- Slider value 가 1에 가까워질수록 금속 텍스처에 가까워지는 특성이 있음
![](https://i.imgur.com/WFyLaBe.gif)

Normal Map (노멀 맵)
--

- 표면의 세밀함 입체감이나 질감을 표현하기 위한 텍스처의 일종
- 3D 모델링으로 폴리곤을 소모하지 않고 같은 효과를 낼 수 있음

|노멀 맵 1|노멀 맵 10|
|:--:|:--:|
|<img src="https://i.imgur.com/VdXmiVv.png" width="400">|<img src="https://i.imgur.com/nv6xSWo.png" width="400">|


Height Map (하이트 맵)
--

- 텍스처 높낮이를 표현
- 노멀 맵과 비슷하지만 좀 더 돌출시켜 뒤에 있는 오브젝트를 가리는 '오클루전(Occlusion)' 효과를 낼 수 있음
![](https://i.imgur.com/sGnUD4A.png)

Occlusion (오클루전)
--

- 흑백 텍스처로 간접조명에 의새 생기는 명암을 더욱 뚜렷이 표시해 사물의 입체감과 깊이감을 살리는데 사용
- 일반적으로 3D 모델링 툴 또는 서드파티 툴에서 추출함

Emission (이미션)
--

- 스스로 빛을 방출하는 속성
- 속성값을 변경하면 객체의 표면에서 방출되는 빛의 강도 및 색상을 설정할 수 있음

|이미션 false|이미션 true|
|:--:|:--:|
|![](https://i.imgur.com/AjPd7zr.png)|![](https://i.imgur.com/ytOx2sR.png)|


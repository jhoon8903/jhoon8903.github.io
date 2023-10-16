---
title: Unity Light Probe 라이트 프로브
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/xKT92Ud.gif
comments_disable: true
meta_description: Unity Light Probe 라이트 프로브
---
# Light Probe (라이트 프로브)
- 동적으로 움직이는 객체에는 [[2023-10-16-5-Light_Mapping|라이트매핑]]을 통한 광원 조명효과를 적용할 수 없음
- 라이트매핑을 static Object 에만 영향을 미치기 때문에 Light Probe를 지원함
- 라이트 프로브는 스테이지의 조명이 있는 곳 주변에 라이트 프로브를 배치하고, 라이트맵을 베이킹할 때 해당 라이트 프로브에 주변부 광원 데이터를 미리 저장해둠
- 라이트 프로브에 저장된 광원 데이터는 실행 시 근처를 지나치는 Dynamic Object에 광원 데이터를 전달해 해당 객체릐 색상과 보간시켜 `마치 실시간 조명과 같은 효과`를 내는 방식

![](https://i.imgur.com/hEgGrdF.jpg)

![](https://i.imgur.com/RueZjCP.jpg)
🔆 플레이어 케릭터는 Dynamic Object이지만 Realtime Light 와 같은 연출이 가능해짐

## Light Probe Group

![](https://i.imgur.com/zlpYs1O.jpg)

![](https://i.imgur.com/ng1rW3n.jpg)

![](https://i.imgur.com/hYfT5z4.jpg)

- 8개의 구체가 Light Probe
- 라이트 프로브의 경우 항상 바닥 위에 위치해야 효과를 낼 수 있음

##### Light Probe 버튼
|버튼 명|설명|
|:--|:--|
|Add Probe|새로운 Light Probe를 생성|
|Delete Selected|선택한 Light Probe를 삭제|
|Select All|현재 Scene View에 있는 모든 Light Probe를 선택한다|
|Duplicate Selected|선택된 Light Probe를 복제<br>여러개를 복제할 수 있다|

![](https://i.imgur.com/xKT92Ud.gif)
## Anchor Override

![](https://i.imgur.com/WKYzkGx.jpg)

![](https://i.imgur.com/USUXlkY.jpg)

- 라이트 프로브는 Objcect의 가장 근접한 4개의 라이트 프로브에서 베이크된 조명 값을 전달
- Anchor Override 속성을 이용해 보간되는 위치를 설정할 수 있음
- 원하는 Transform을 연결하면 라이트 프로브가 조명 값을 전달하는 위치가 Transform 쪽으로 변경 됨
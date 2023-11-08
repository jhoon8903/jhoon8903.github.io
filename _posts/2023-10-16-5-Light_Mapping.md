---
title: Unity 라이트매핑
category: Unity
author: 이정훈
tags: [Unity, Lighting]
img: https://i.imgur.com/0qJ6Od4.jpg
comments_disable: true
meta_description: Unity 라이트매핑
---
# 라이트매핑
- Scene에 배치괸 모든 3D Object Model에 영향을 미치는 직접 조명, 간접 조명 및 그림자의 효과를 Texture로 미리 만드는 과정
- 결과로 만들어진 Texture File을 `Lightmap`이라고 함
- Lightmap을 만드는 과정을 `Bake`라고 함
- Lightmap은 Runtime 시 실시간 Rendering 화면과 Overlay되어 Mixing 됨
- Runtime에 전역 조명에 대한 연산처리를 하지 않고도 높은 품질의 조명 효과를 구현할 수 있음

## Generate Lightmap UVs 옵션
- 외부에서 Import한 3D 모델의 경우 `Generate Lightmap UVs`를 설정해야 함
![](https://i.imgur.com/0qJ6Od4.jpg)

## Contribute GI Flag
- Light Mapping을 하려면 Light Mapper에 그 대상을 알려주어야 함
![](https://i.imgur.com/eDnefnn.jpg)
❗️ Contribute GI Flag를 체크하지 않으면 Lightmapping에서 제외 됨

## Light View 속성

![](https://i.imgur.com/PxwnRxl.jpg)

##### Light View (Scene) 
|속성|설명|
|:--|:--|
|Lightng Settings|라이트 매핑 정보를 저장하는 에셋을 지정|
|Realtime Lighting|실시간 전역 조명 기능의 활성화 여부를 결정|
|Mixed Lighting|혼합 조명(Mixed Lighting) 기능의 활성화 여부를 결정<br>- Baked Global Illumination : 전역 조명을 Bake 할 것인지 결정하는 옵션<br>- Lighting Mode : [[2023-10-16-4-Lighting]] 참조|
|Lightmapping Setting|라이트매퍼의 주요 옵션 설정<br>- Progressive / Enlighten 라이트매퍼를 선택<br>- 직접 광원 / 간접 광원의 품질 설정<br>- 간접 광원의 Bounce 설정<br>- 라이트맵의 텍셀(Texel)설정(라이트맵의 해상도 설정)<br>- 라이트맵의 텍스처 사이즈 설정<br>- 라이트맵의 압축 기능 사용 여부 설정(압축시 퀄리티가 떨어질 수 있음)<br>- Ambient Occlusion 효과의 적용 여부|

##### Light View (Environment)

![](https://i.imgur.com/jFcTEP6.jpg)

|속성|설명|
|:--|:--|
|Environment|환경 광원에 대한 설정<br>- 스카이박스와 태양 광원의 설정<br>- 실시간 그림자 색상 설정<br>- 환경 광원(Ambient Light)의 소스, 조도, 베이크 모드를 설정하는 옵션<br>- 반사 광원(Reflection Light)의 소스, 조도, 압축 등을 설정하는 옵션|
|Other Setting|기타 설정 옵션<br>- 안개 효과의 사용 여부<br>- 헤일로(Halo) 효과의 사용 여부<br>- 플레어(Flare) 효과의 속성 설정<br>- 라이트에 적용할 쿠키(Cookie)를 지정하는 속성|

### ❗️Auto Generate
- Auto Generate 체크 시 백그라운드로 라이트맵을 베이크 할 수 있음
- Scene View에서 3D 모델, 조명을 배치하거나 위치를 변경하면 Lightmapper가 자동으로 Bake 됨
![](https://i.imgur.com/Xzn4ZkY.jpg)
- Bake 된 라이트맵은 Cache에 저장 됨
- Unity => Edit => Preferences => GI Cache에서 경로 및 사이즈 설정
![](https://i.imgur.com/8998l0o.jpg)
❕ 실행 파일에 라이트맵을 함께 배포하려면 `반드시 Auto Generate 옵션을 끄고 수동으로 베이크 해야함`

### Ambient Occlusion
- 명암 대비를 크게 해 입체감을 높이는 속성

![](https://i.imgur.com/hjldql4.jpg)

![](https://i.imgur.com/b8leRJb.jpg)

|Ambient Occlusion의 세부 속성|설명|
|:--|:--|
|Max Distance|Ambient Occlusion의 범위를 결정하는 속성|
|Indirect Contribution|간접 광원에 대한 명암비를 결정하는 속성|
|Direct Contribution|직접 광원에 대한 명암비를 결정하는 속성|

##### Bake 된 Lightmap
![](https://i.imgur.com/Qdsww1i.jpg)

## Area Light
- 실시간 조명이 아니기 때문에, 라이트 매핑 때만 조명 효과가 표시 됨
- 특히, 간접 조정 효과를 내는 데 효과적
![](https://i.imgur.com/v4nNLIe.jpg)


![](https://i.imgur.com/oTRxvFa.jpg)

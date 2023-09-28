---
title: Unity Post Processing
category: Unity
author: 이정훈
tags:
  - unity
img: 
comments_disable: true
meta_description: Unity Post Processing
---

# Post Processing

- 포스트 프로세싱 : 후처리
- 게임 화면이 최종 출력되기 전에 카메라의 이미지 버퍼에 삽입

하는 추가 처리

|프로세싱 전|프로세싱 후|
|:--:|:--:|
|![](https://i.imgur.com/x73veg3.jpg)|![](https://i.imgur.com/ron0pZd.jpg)|

- 대부분의 포스트 프로세싱 연산은 렌더링 파이프라인의 주요 과정에서 적용되 않고 마지막 부분에 적용 됨
- Post Processing Stack 패키지 제공


## Rendering Path (렌더링 경로)

![](https://i.imgur.com/aCsBwV2.png)

- Rendering Path 는 렌더링이 처리되는 순서와 방법을 결정하는 옵션
- 기본 값인 Use Graphics Setting은 프로젝트 설정에 맞추어 렌더링 경로를 결정
- 일반적으로 포워드 렌더링 Forward Rendering 옵션으로 설정

### Forward
- 각각의 오브젝트를 그릴 때마다 해당 오브젝트에 영향을 주는 모든 라이팅도 함께 계산하는 전통적인 방식
- 메모리 사용향이 적어 저사양에서도 비교적 잘 동작
- 연산이 느림
- 오브젝트와 고아우너이 움직이거나 수가 많아질수록 연산량이 급증
- 하나의 오브젝트에 최대 4개의 광원만 제대로 개별 연산
- 나머지 중요하지 않은 광원과 라이팅 효과는 부하를 줄이기 위해 합쳐서 한번에 연산
- 실제와 다르게 표현될 수 있음

### Deferred
- 라이팅 연산을 `미뤄서`실행하는 방식
- 첫 번째 패스에서는 오브젝트의 메시를 그리되 라이팅을 계산하거나 색을 채우지 않고 버퍼에 저장
- 두 번째 패스에서 첫 번째 패스의 정보를 활용하여 라이팅을 계산하고 최종 컬러를 결정
- 개수 제한 없이 광원을 표현
- 모든 광원의 효과가 올바르게 적용 됨
- 단, MSAA 같은 일부 안티앨리어싱(계단현상제거) 설정울 제대로 지원하지 않음

## 프로세싱 적용

- 컴포넌트 Post-process Layer 추가
![](https://i.imgur.com/YbFlMlI.png)

---
title: Unity 동적장애물 - NavMeshObstacle
category: Unity
author: 이정훈
tags: [Unity, Navigation]
img: https://i.imgur.com/QYnDxe1.gif
comments_disable: true
meta_description: Unity 동적장애물 - NavMeshObstacle
---
# NavMeshObstacle

- 처음 Bake 된 장애물의 경우 장애물이 사라지거나 이동 되었을 때 해당 위치는 Nav에서 장애물로 인식
- 동적으로 변경 되는 장애물의 경우 NavMeshObstacle 컴포넌트로 해결

![](https://i.imgur.com/ErBeGhe.jpg)


![](https://i.imgur.com/Ksj4dh7.jpg)

### Carve 옵션
- 실시간으로 NavMesh가 변경
- 부하가 크기 때문에 최적화에 신경 써야 함

![](https://i.imgur.com/QYnDxe1.gif)

|Carve 옵션|설명|
|:--|:--|
|Move Threshold|속성값의 거리만큼 이동했을 때 내비메시를 갱신|
|Time To Stationary|동일 위치에서 일정 시간 동안 정지했을 때 NavMesh를 갱신|
|Carve Only Stationary|정지 상태에만 NavMesh를 갱신|

## NavMeshAgent 함수

|속성|설명|
|:--|:--|
|updatePosition|위치를 자동으로 이동시키는 옵셥|
|updateRotation|자동으로 회전시키는 옵션|
|remainingDistance|목적지까지 남은 거리|
|velocity|에이전트의 현재 속도|
|desireVelocity|장애물 회피를 고려한 이동 방향|
|pathPending|목적지까지의 최단거리 계산이 완료됐는지 여부|
|isPathStale|계산한 경로의 유효성 여부(동적 장애물, OffMeshLink)|

## Area Mask

![](https://i.imgur.com/leVzmlJ.jpg)

Cost에 따라서 적은 Cost 방향으로 AI Nav가 이동함
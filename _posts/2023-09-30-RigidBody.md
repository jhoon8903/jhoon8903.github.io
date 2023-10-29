---
title: Unity RigidBody
category: Unity
author: 이정훈
tags: [Unity, Physic]
img: https://i.imgur.com/bCgTfRP.jpg
comments_disable: true
meta_description: Unity RigidBody
---
# RigidBody

- 물리학 용어 : 외력을 가해도 크기나 형태가 변하지 않는 이상적인 물체
- 아주 큰 힘을 받지 않는 한 부서지지 않는 대부분의 고체는 강첼로 간주한다.

![](https://i.imgur.com/bCgTfRP.jpg)

## Mass (질량 float Type)
- 상대적인 의미의 질량으로 1Kg, 1g 등의 무게가 아님
- 편의상 1Kg으로 가정
## Drag (이동 마찰 계수 float Type)
- 이동할 때 적용되는 마찰계수 (저항)
## Angular Drag (회전 마찰 계수 float Type) 
- 회전할 때 적용되는 마찰계수 (저항)
## Use Gravity (중력 bool Type)
- 중력의 적용 여부
## Is Kinematic (물리엔진 적용 여부 bool Type)
- true 일때, 물리 시뮬레이션의 영향을 받지 않고, transform 컴포넌트를 이용해 이동
## Interpolate (물리력 보간 Select Type)
![](https://i.imgur.com/smJOqSP.jpg)
- 물리력을 이용한 움지김이 끊어지는 현상이 발생할 때 보간 (Interpolate)
#### Interpolate : 이전 프레임의 Transform에 맞게 움직임을 부드럽게 보간 처리
#### Extrapolate : 다음 프레임 Transform 변화를 추정해 움직임을 부드럽게 처리
## Collision Detection (충돌 감지 Select Type)
- CCD : Conituous collison detection
![](https://i.imgur.com/ThX40vQ.jpg)
- 아주 빠른 물체는 물리 엔진이 충돌 검출을 놓칠 수 있음
- 좀 더 세밀한 충돌을 검출하기 위한 옵션
### Discrete (이산)
	- 기본적인 충돌 감지 방식
	- 프레임마다 한 번씩 충돌을 확인
	- 빠르게 움직이는 객체의 경우에는 "터널링"문제가 발생할 수 있음
#### ❗️"터널링" : 객체가 빠르게 움직여 프레임에서 프레임으로 아동하는 동안 다른 객체를 완전 통과하는 형상
### Continuous (연속)
	- 가장 정확한 충돌 감지 방식
	- 객체의 이동 경로를 통해 프레임 사이의 모든 가능한 충돌을 감지
	- 빠르게 움직이는 객체에 주로 사용
### Continuous Dynamic (연속 동적)
	- 연속 충돌 감지는 동젝 객체에만 적용 (Continuous )
	- 정적(static), Kinematic 객체에 대해서는 이산 충돌 감지 적용 (Discrete)
### Continuous Speculative (연속 추측)
	- 성능 최적화를 위해 추가된 방식
	- 실제 이동보다 약간 앞선 위치에서의 예측된 충돌을 기반으로 감지
	- 일반적으로 터널링 문제를 방지 및 높은 성능 유지를 위해 사용
#### 💥 총알과 같은 객체는 Continuous or Continuous Speculative 를 적용

## Freeze Position 
x, y, z 축 중에서 해당 축으로의 이동을 막음

## Freeze Rotation
x, y, z 축 중에서 해당 축을 기준으로 회전을 막음

## AddForce / AddRelativeForce
```csharp
void AddForce(Vector3 force);
void AddRelativeForce(Vector3 force);
```
- 두 함수 모두 Rigidbody 컴포넌트를 받아 실행할 수 있음
- AddForce의 경우 월드 좌표 기준으로 힘을 가함
- AddRelativeForce의 경우 로컬 좌표 기준으로 힘을 가함

```csharp
// 월드 좌표 AddForce
_rb.AddForce(transform.forward * force);

// 로컬 좌표계 AddRealativeforce
GetComponent<RigidBody>().AddRelativeForce(Vector3.forward * force);
```
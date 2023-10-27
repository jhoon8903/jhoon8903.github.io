---
title: Unity Navigation
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/zneH4to.gif
comments_disable: true
meta_description: Unity Navigation
---
# Navigation

- 중간에 장애물이 있거나 다른 충돌 및 경합을 벌이는 경우 길 찾기(Navigation)기능이 필요

##### A* Pathfinding 알고리즘 (가장 많이 쓰이는 네비게이션 에셋)
![](https://i.imgur.com/oFJgaVG.jpg)

## 유니티에서 제공하는 네비게이션 방식
- 스테이지를 구성하고 있는 3D Mesh (지형 : Geometry)를 분석, NavMesh 데이터를 미리 생성하는 방식
- 추적할 수 있는 영역(Walkable Area)과 장애물로 판단해 지나갈 수 없는 영역(Non Walkable Area)의 <br>데이터를 Mesh 로 미리 만드는 것
- 높은 곳에서 낮은 곳으로 연결하는 오프 메시 링크(Off Mesh Link) 기능을 이용해 뛰어 내리는 동작 연출

## Navigation Static Flag 네비게이션 설정
### 🔆 Unity 2022.03 이상에서 기존 Static Navigation 지원 중단
- 절대강좌! 유니티 2021 내용 실행 불가
- ❗️Package Manager 에서 AI Navigation 설치
![](https://i.imgur.com/wSrz5QF.jpg)

### 1. NavMesh Surface 하이어라키 추가
![](https://i.imgur.com/sQE4clA.jpg)

### 2. Nav Bake
![](https://i.imgur.com/mZruTJM.jpg)
 
![](https://i.imgur.com/IU1ulv6.jpg)

## NavMeshAgent Component
- NavMesh Data를 기반으로 목적지까지 최간 거리를 계산해 이동하는 역할
- 장애물과 다른 NPC 간의 충돌을 회피하는 기능 제공
- A* Pathfinding 알고리즘을 사용
![](https://i.imgur.com/ripCCDg.jpg)

|속성|설명|
|:--:|:--|
|Agent Type|장애물을 회피하기 위한 회전반경 및 넘어갈 수 있는 계단의 높이, 경사로 등<br>판 각도의 정보를 정의한 것을 Agent라고 한다.<br>기본값은 Humanoid로 지정돼 있으며, 개발자가 추가로 정의해 지정 가능|
|Base Offset|NavMeshAgent는 Bake된 NavMesh Surface에 붙어 이동<br>이때, 캐릭터가 바닥에서 뜬 상태로 이동하는 것처럼 보이는 것을 수정하기 위해 NavMeshAgent의 높이 조절|
|Speed|최대 이동 속도 (초당 월드 단위)|
|Angular Speed|최대 회전 속도 (초당 각도)|
|Acceleration|최대 가속(m/s^2)|
|Stopping Distance|목표 지점에 가까워졌을 때 정지하는 근접 거리|
|Auto Braking|목표 지점에 가까워졌을때 속도를 줄이는 기능으로 순찰과 같이<br>여러 포인트를 부드럽게 이동하기 위해서는 이 기능을 비활성화해야 한다.|
|Radius|Agent의 반경으로 장애물을 크게 돌아갈 것이지, 짧게 돌아갈 것인지 결정<br>다른 Agent간의 충돌을 계산하기 위해 사용된다.|
|Height|Agent가 머리 위에 있는 장애물을 지나갈 때의 높이를 의미<br>이 수치가 클수록 넘어갈 수 있는 계단의 스텝(step)도 커진다.|
|Quality|장애물을 회피할 때의 품질을 설정하는 것으로 많은 에이전트가 있으면 품질을 낮춰서 CPU의 부하를 줄임|
|Priority|Agent간의 회피 우선순위를 결정<br>자신보다 낮은 에이전트는 회피대상에서 제외<br>0 ~ 99 사이의 값을 가지며 낮은 수가 높은 우선순위|
|Auto Traverse Off Mesh Link|분리된 Mesh 간에 자동으로 링크를 생성하는 옵션<br>Off Mesh Link 컴포넌트를 이용해 직접 링크를 만들 때 이 기능은 비활성화|
|Auto Repath|활성화시 이동할 경로가 유효하지 않은 때 다시 경로를 재탐색|
|Area Mask|Area Mask를 지정한 후 NavMesh를 Bake 하면 특정 영역별로 이동을 제한할 수 있는 기능<br>특히, RunTime에서 비트 마스크를 통해 조작 가능|

지정 된 목표에 대한 추적 실행
![](https://i.imgur.com/zneH4to.gif)

## Animator Params
|Set Function|Get Function|
|:--|:--|
|Animator.SetBool|Animator.GetBool|
|Animator.SetFloat|Animator.GetFloat|
|Animator.SetInterger|Animator.GetInterger|
|Animator.SetTrigger|Animator.GetTrigger|

```csharp
// Animator의 String 값 Hash Load
private static readonly int IsTrace = Animator.StringToHash("IsTrace");  

private IEnumerator MonsterAction()  
{  
	while (!isDie)  
	{  
		switch (state)  
		{  
			case State.IDEL:  
				_agent.isStopped = true;  
				// Animator의 IsTrace 값을 false로 변경  
				_anim.SetBool(IsTrace, false);  
				break;  
			case State.TRACE:  
				_agent.SetDestination(_playerTr.position);  
				_agent.isStopped = false;  
				_anim.SetBool(IsTrace, true);  
				break;  
			case State.ATTACK:  
				break;  
			case State.DIE:  
				break;  
		}  
		yield return new WaitForSeconds(0.3f);  
	}  
}
```

![](https://i.imgur.com/JKTyXD0.jpg)


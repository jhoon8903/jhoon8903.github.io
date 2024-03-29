---
layout: post
title: Unity Physics Manager
subtitle: Unity 물리효과를 세팅라는 Physics Manager
categories: Unity
author: Daniel
tags: 
 - Physic
 - Unity
banner:
 image: https://i.imgur.com/PrhvfgU.jpg
---

Physics Manager 
--

![](https://i.imgur.com/PrhvfgU.jpg)

## Gravity (중력) - Vector3 Type
![](https://i.imgur.com/8f4SPdN.jpg)
- Rigidbody의 `Use Gravity` 속성에 체크하면, 중력에 설정된 값으로 중력이 작용하게 됩니다.
- 일반적으로 지구의 중력 가속도와 같은 `-9.81`의 값이 Y축에 적용됩니다.
- 예를 들어, 우주 환경과 같이 중력이 약한 환경에서는 이 값을 줄여서 적용할 수 있습니다.


---

## Default Material (기본 Material) - PhysicMaterial Type
![](https://i.imgur.com/tMxaayN.jpg)
- 두 물체가 충돌했을 때의 물리적 반응(탄성, 마찰 등)을 전역적으로 설정하는 Material입니다.
- `None`으로 설정하면, 각 Rigidbody에서 해당 물체의 특성에 맞게 개별적으로 물리적 반응을 설정할 수 있습니다.


---

## Bounce Threshold (반사 임계값) - float Type
![](https://i.imgur.com/ywEhYgF.jpg)
- 두 객체가 서로 충돌할 때, 상대적인 속도가 어느 정도 이상일 때 반사가 발생하는지를 정의하는 임계값입니다.
- 만약 충돌한 두 물체의 상대 속도가 이 값보다 작다면, 물체는 반사되지 않고 멈추게 됩니다.
- 이는 물체 간의 떨림 현상(Jitter)을 감소시키는 데 도움을 줍니다.


---

## Default Max Depenetration Velocity (기본 최대 중첩 해결 속도) - float Type
![](https://i.imgur.com/GqnkxzE.jpg)
- 물리 엔진에서 두 물체가 서로 중첩(겹침)되었을 때, 그 중첩을 해결하기 위해 물체에 적용될 수 있는 최대 속도를 정의하는 옵션
- 실제 게임 또는 시뮬레이션에서 때때로 두 물체가 충돌할 때 그들이 약간 서로 겹칠 수 있음
- 겹치는 것을 "프네트레이션(Penetration)" 또는 "딥네트레이션(Depenetration)"이라고 함
- 물리 엔진은 두 물체를 가능한 빠르게 분리하여 겹침을 해결하려고 시도
- 이 값이 높으면 겹침이 감지될 때 물체가 더 빠른 속도로 튕겨져 나갈 수 있음
- 이 값이 낮으면 물체가 더 천천히 움직여 겹침을 해결하려고 시도


---

## Sleep Threshold (수면 임계값) - float Type
![](https://i.imgur.com/QUZvAYp.jpg)
- Rigidbody가 어느 정도의 작은 움직임 이하로 활동하면 "수면" 상태로 전환되는 임계값입니다.
- 수면 상태의 객체는 물리 계산에서 일시적으로 제외되므로, 더 많은 물체를 효율적으로 처리할 수 있게 되어 성능을 향상시킵니다.


---

## Default Contact Offset (기본 접촉 오프셋) - float Type
![](https://i.imgur.com/goRWUgt.jpg)
- 두 콜라이더가 서로 접촉하게 되기 직전의 거리를 설정하는 값입니다. 
- 이는 미세한 충돌을 미리 감지하도록 도와줍니다.
- 값이 낮게 설정되면, 물체들이 실제로 접촉하기 전에 충돌이 감지되어 의도치 않은 반응이 발생할 수 있습니다.


---

## Default Solver Iterations (기본 솔버 반복) - float Type
![](https://i.imgur.com/KK3px61.jpg)
- 물체간의 충돌 해결을 위한 계산 반복 횟수를 정의합니다.
- 이 값이 높을수록 충돌 해결의 정확도는 올라가지만, 성능에 부담을 줄 수 있습니다.
- 예를 들어, 복잡한 충돌 시나리오에서는 이 값을 높여 충돌 해결의 정확도를 높일 수 있습니다.


---

## Default Solver Velocity Iterations (기본 솔버 속도 반복) - int Type
![](https://i.imgur.com/43NT6WF.jpg)
- 물체의 속도를 고려한 충돌 해결을 위한 계산 반복 횟수를 정의합니다.
- 속도와 관련된 물리적 효과를 정확하게 계산하기 위해 이 값을 조절할 수 있습니다.


---

## Queries Hit Backfaces (쿼리 히트 백페이스) - bool Type
![](https://i.imgur.com/kS6yTOR.jpg)
- 레이캐스트나 다른 물리 쿼리가 콜라이더의 뒷면(백페이스)를 감지할 것인지 여부를 설정합니다.
- 이 옵션은, 예를 들어, 얇은 벽을 뚫고 보는 경우 등 특정 상황에서 필요합니다.


---

## Queries Hit Triggers (쿼리 히트 트리거) - bool Type
![](https://i.imgur.com/gCTTTHo.jpg)
- 레이캐스트나 다른 물리 쿼리가 트리거 콜라이더를 감지할 것인지 여부를 설정합니다.
- 예를 들어, 캐릭터가 특정 영역에 들어갔을 때 이벤트를 발생시키는 경우에 이 옵션을 활용할 수 있습니다.


---

## Enable Adaptive Force (적응형 힘 활성화) - bool Type
![](https://i.imgur.com/dcMYnJZ.jpg)
- 물체의 움직임에 따라 동적으로 힘의 양을 조절하는 기능을 활성화할지 여부를 결정합니다.
- 이는 복잡한 물리 시뮬레이션에서 더 안정적인 결과를 얻기 위해 사용될 수 있습니다.


---

## Contact Generation (접촉 생성) - Enum Type
![](https://i.imgur.com/GcOF0rO.jpg)
- 물체 간 충돌이 발생할 때 Unity의 물리 엔진이 어떻게 충돌 접점을 생성하고 관리할 것인지 결정합니다.
- Unity의 물리 엔진은 효율적인 시뮬레이션을 위해 다양한 방법으로 접촉 점을 관리하며
- 이 중에서도 특히 충돌 시 발생하는 접촉점의 수와 정보를 어떻게 처리할 것인지 결정하는 것이 중요합니다.
##### Legacy Contacts Generation (레거시 접촉 생성)
- 이전 버전의 Unity에서 사용되던 방식으로, 각 충돌마다 새로운 접촉 정보를 생성합니다.
- 매 프레임마다 새로운 접촉 정보가 계산되어, 이전 정보와 독립적으로 작동합니다.
- 더 단순한 계산 방식이지만, 일관성이나 최적화 면에서 현대적인 방식보다 떨어질 수 있습니다.
##### Persistent Contacts Manifold (지속적 접촉 공간)
- 물체 간의 충돌 시 발생하는 접촉 정보를 일정 시간 동안 지속적으로 유지하고 관리하는 방식입니다.
- 이 방식은 물체 간의 접촉 정보를 연속적으로 추적, 더욱 안정적이고 자연스러운 반응을 만들어낼 수 있습니다.
- 연속적인 충돌 상황, 예를 들어 물체가 다른 물체에 계속해서 접촉하고 있는 상황에서 더 효율적입니다.
- 최신 버전의 Unity에서 권장되는 방식입니다.


---

## Simulation Mode (시뮬레이션 모드) - Enum Type
![](https://i.imgur.com/EucH2MI.jpg)
- 물리 시뮬레이션의 갱신 주기를 결정하는 옵션
- 게임 또는 App의 특성에 따라 모드를 선택
##### Fixed Update(고정 갱신)
- 물리 시뮬레이션은 정해진 간격으로 일정하게 업데이트 됨
- Fixed Update 시간 간격 조정 (Update의 경우 구동 환경에 따라 달라짐)
- 물리적인 상호작용이 중요한 게임 (레이싱 게임등)에서 권장하는 옵션
- 물리 계산의 일관성을 위해 고정된 간격으로 갱신되기 때문에 물리적 버그나 예기치 않은 행동을 최소화하는데 도움
##### Update(갱신)
- 물리 시뮬레이션은 매 프레임마다 갱신
- 프레임 레이트에 따라 갱신 주기가 달라질 수 있어, 물리 반응이 일정하지 않을 수 있음
- 빠르게 프로토타이핑하거나 물리 계산의 정확성이 크게 중요하지 않은 경우에 사용
##### Script(스크립트)
- 개발자가 직접 스크립트를 통해 물리 시뮬레이션 갱신 시기를 제어할 수 있음
- 특정한 상황 또는 조건에서만 물리 시뮬레이션을 진행하고 싶을 때 유용
- 예를 들면, 특정 이벤트가 발생했을 때만 물리 계산을 시작하는 등의 커스텀 로직을 구현할 수 있음


---

## Auto Sync Transforms (자동 동기화 변환) - Bool Type
![](https://i.imgur.com/FgwEmAr.jpg)
- 물리 시뮬레이션 후에 물체의 변환 정보를 자동으로 업데이트할지 결정하는 옵션입니다.
- 이를 활성화하면 시뮬레이션 후에 자동으로 Transform 컴포넌트가 업데이트됩니다.


---

## Reuse Collision Callbacks (충돌 콜백 재사용) - Bool Type
![](https://i.imgur.com/4Bv6Vhz.jpg)
- 물체 간의 지속적인 충돌 상황에서, 이전 프레임의 충돌 정보를 재사용할지 여부를 결정합니다.
- 이를 통해 물리 계산의 성능을 향상시킬 수 있습니다.


---

## Invoke Collision Callbacks (충돌 콜백 호출) - Bool Type
![](https://i.imgur.com/zQ5FPnO.jpg)
- 물체 간 충돌 시, 해당 콜백 함수를 호출할지의 여부를 결정합니다.
- 이 옵션을 활성화하면 물체 간의 충돌 시 Unity의 콜백 함수가 호출됩니다.


---

## Cloth Gravity (옷 중력) - Vector3 Type
![](https://i.imgur.com/49wBlXB.jpg)
- 옷 시뮬레이션에 적용되는 중력 값을 정의합니다.
- 일반적인 물리 중력과 다르게 옷에만 특별히 적용되는 중력을 설정할 수 있습니다.


---

## Contact Pairs Mode (접촉 쌍 모드) - Enum Type
![](https://i.imgur.com/8Q44njH.jpg)
- 물체 간의 충돌 쌍을 어떻게 처리할지 결정하는 모드입니다.
- 이를 통해 복잡한 충돌 상황에서의 성능과 정확도를 조절할 수 있습니다.
##### Default Contact Pairs (기본 접촉 쌍)
- Unity의 물리 엔진이 기본적으로 제공하는 충돌 처리 방식을 사용합니다.
- 일반적인 충돌에 대한 표준 접촉 정보가 생성됩니다.
##### Enable Kinematic Kinematic Pairs (키네매틱-키네매틱 쌍 활성화)
- 두 키네매틱 Rigidbody 간의 충돌 접촉 정보를 생성합니다.
- 일반적으로 키네매틱 Rigidbody는 다른 키네매틱 Rigidbody와의 충돌 접촉 정보를 생성하지 않지만, 이 옵션을 활성화하면 그렇게 할 수 있습니다.
##### Enable Kinematic Static Pairs (키네매틱-스태틱 쌍 활성화)
- 키네매틱 Rigidbody와 스태틱 Collider 간의 충돌 접촉 정보를 생성합니다.
- 일반적으로 키네매틱 Rigidbody는 스태틱 Collider와의 충돌 접촉 정보를 생성하지 않지만, 이 옵션을 활성화하면 그렇게 할 수 있습니다.
##### Enable All Contact pairs (모든 접촉 쌍 활성화)
- 모든 가능한 Rigidbody와 Collider 조합 간의 충돌 접촉 정보를 생성합니다.
- 이는 게임 내에서 모든 종류의 충돌 감지와 반응이 필요할 때 유용하게 사용됩니다.


---

## Broadphase Type (브로드페이즈 타입) - Enum Type 
![](https://i.imgur.com/A5lA9t3.jpg)
- 물리 시뮬레이션에서 두 물체가 충돌할 가능성이 있는지 빠르게 확인하는 초기 단계를 말합니다. 
- 이 단계는 실제로 두 물체가 정확하게 어떻게 충돌하는지(페이즈 단계)를 계산하기 전에, 큰 범위에서 어떤 물체들이 가능성 있는 충돌 쌍인지를 식별하는 데 중점을 둡니다.
##### Sweep And Prune Broadphase (스위프트 앤 프룬 브로드페이즈)
- 각 물체에 대한 경계 상자를 생성하고, 일렬로 정렬된 축(보통 X 또는 Y 축)을 따라 "스윕"합니다.
	❓ 스윕 : 훑기, 스캔하기
- 정렬 과정에서 경계 상자가 겹치는 경우, 해당 물체들이 충돌할 가능성이 있다고 판단합니다.
- 일반적으로 동적 환경에서 효과적이며, 물체들이 자주 움직이는 경우에도 높은 성능을 보입니다.
##### Multibox Pruning Broadphase (멀티박스 프루닝 브로드페이즈)
- 물체들의 여러 경계 상자를 생성, 이러한 상자들 사이의 가능한 충돌을 빠르게 식별하기 위한 알고리즘입니다.
- 복잡한 환경에서도 빠르게 가능한 충돌 쌍을 식별할 수 있습니다.
- 특히, 여러 물체가 겹치는 복잡한 시나리오에서 효율적입니다.
##### Automatic Box Pruning (자동 박스 프루닝)
- 물체들의 경계 상자를 자동으로 생성하고, 이를 기반으로 가능한 충돌 쌍을 식별합니다.
- 상자의 크기와 위치는 물체들의 동작과 환경에 따라 자동으로 조정됩니다.
- 다양한 환경과 상황에서 광범위하게 사용, 특별한 최적화 없이도 일반적으로 높은 성능을 제공합니다.


---

## World Bounds (월드 경계) - Bounds Type
![](https://i.imgur.com/P0hd90f.jpg)
- 브로드페이즈 충돌 검출의 최적화를 위해 사용되는 세계의 경계를 정의합니다.
- 이 경계 안에서의 물체만 브로드페이즈 검출 대상으로 포함됩니다.
##### Center (중심) - Vector3 Type
- 월드 경계의 중심점을 나타냅니다. 
- 이 값은 월드 좌표계에 기반하여 설정됩니다. 
- 월드 경계의 위치를 조절하는 데 사용됩니다.
##### Extent (범위 또는 확장) - Vector3 Type
- 월드 경계의 크기의 절반을 나타냅니다. 
- 예를 들면, Extent 값이 (10, 10, 10)이면, 월드 경계의 전체 크기는 X, Y, Z 축 각각 20 유닛입니다.
- 이 값은 월드 경계의 크기를 조절하는 데 사용됩니다.

- 월드 경계는 주로 성능 최적화를 위한 목적으로 사용됩니다. 
- 객체가 월드 경계를 벗어나면 그 객체에 대한 물리 계산을 수행하지 않을 수 있어, 시뮬레이션 성능을 향상시킬 수 있습니다. 
- 따라서 월드 경계의 크기와 위치는 게임의 씬에 맞게 적절히 조정되어야 합니다.


---

## World Subdivisions (월드 세분화) - Int Type
![](https://i.imgur.com/ab3RRfh.jpg)
- 월드 경계를 몇 개의 하위 구역으로 나눌 것인지 결정합니다.
- 더 많은 세분화는 세밀한 충돌 검출에 도움이 되지만, 성능에 영향을 줄 수 있습니다.


---

## Friction Type (마찰 타입) - Enum Type
![](https://i.imgur.com/bQgTPSz.jpg)
- 물체 간의 마찰 효과를 어떤 방식으로 처리할 것인지 결정하는 타입입니다.
##### Patch Friction Type (패치 마찰 타입)
- 이 타입은 두 물체 간의 여러 접촉 점들을 하나의 접촉 패치로 그룹화합니다.
- 패치 기반 마찰은 물체의 전체 접촉 영역을 고려하여 마찰을 계산합니다.
- 이 방법은 리얼타임 게임에서 자주 사용되며, 물체의 전체 접촉 영역에 대한 평균 마찰 효과를 제공합니다.
##### One Directional Friction Type (일방향 마찰 타입)
- 이 타입은 접촉 영역의 주된 방향으로만 마찰을 계산합니다.
- 주로 물체의 움직임 방향에 대한 저항만을 고려할 때 사용됩니다.
- 예를 들어, 빠르게 움직이는 객체가 다른 객체와 부딪힐 때, 그 움직임의 방향에 주로 영향을 받는 마찰을 시뮬레이션하는데 적합합니다.
##### Two Directional Friction Type (양방향 마찰 타입)
- 이 타입은 두 개의 주요 방향에 대해 마찰을 계산합니다.
- 이는 물체가 두 개의 주요 축 방향으로 동시에 움직일 때 유용합니다.
- 예를 들어, 자동차가 길을 따라 움직이면서 동시에 좌우로 회전하는 경우, 양방향 마찰 타입이 적절하게 마찰을 처리합니다.


---

## Enable Enhanced Determinations (향상된 결정 사용 활성화) -  Bool Type
![](https://i.imgur.com/U2EvxM2.jpg)
- 물리 시뮬레이션의 일관성과 예측 가능성을 향상시키기 위해 향상된 결정 알고리즘을 사용할 것인지 여부를 설정합니다.


---

## Enable Unified Heightmaps (통합 높이맵 사용 활성화) - Bool Type
![](https://i.imgur.com/wGS9LCV.jpg)
- 지형의 높이맵 데이터를 통합하여 처리할 것인지의 여부를 결정합니다.
- 이 옵션을 활성화하면 높이맵 기반의 지형 처리에 최적화할 수 있습니다.


---

## Improved Patch Friction (개선된 패치 마찰) - Bool Type
![](https://i.imgur.com/ULtEDdJ.jpg)
- 물체 간의 마찰을 더 자연스럽고 정확하게 처리하기 위한 알고리즘을 사용할 것인지 결정합니다.


---

## Solver Type (솔버 타입) -  Enum Type
![](https://i.imgur.com/0xMSEM9.jpg)
- 솔버는 물리 엔진에서 물체 간의 충돌, 물체와 환경 간의 충돌을 해결하기 위해 사용되는 계산 방법
- 솔버 타입에 따라 충돌의 반응, 안정성, 계산 속도 등이 달라질 수 있음
##### Projected Gauss Seidel (프로젝티드 가우스-제이델)
- **기본적인 아이디어:** Gauss Seidel 방법은 반복적인 방법으로 선형 시스템의 해를 찾는 알고리즘입니다.
- "Projected"는 이 방법을 비선형 제약 조건에 적용할 수 있게 확장한 것을 의미합니다.
- **장점:** 안정성이 뛰어나며, 여러 충돌을 동시에 처리하는 복잡한 시나리오에서도 좋은 결과를 제공합니다.
- **단점:** 계산량이 많을 수 있어, 매우 많은 물체가 동시에 움직일 때의 성능에 영향을 줄 수 있습니다.
- **적용 사례:** 복잡한 시뮬레이션, 여러 물체 간의 연속적인 충돌이 발생하는 경우에 사용됩니다.
##### Temporal Gauss Seidel (템포럴 가우스-제이델)
- **기본적인 아이디어:** 이 방법은 시간적 연속성을 고려하여 충돌을 해결하는 방식입니다. 
- 기존의 Gauss Seidel 방법을 시간 축에서의 연속성에 맞게 확장한 것입니다.
- **장점:** 빠른 계산 속도와 시간에 따른 충돌의 연속성을 잘 반영할 수 있습니다.
- **단점:** Projected Gauss Seidel에 비해 약간의 정확성을 희생할 수 있습니다.
- **적용 사례:** 실시간 게임, 빠른 반응이 필요한 애플리케이션에서 주로 사용됩니다.


---

## Default Max Angular Speed (기본 최대 각속도) - Float Type
![](https://i.imgur.com/ILeu2GY.jpg)
- 물체의 회전하는 움직임에 적용되는 최대 각속도를 정의합니다.


---

## Layer Collision Matrix (레이어 충돌 매트릭스) - Matrix Type
![](https://i.imgur.com/yHxQXu9.jpg)
- 서로 다른 레이어 간의 충돌 가능성을 결정하는 매트릭스입니다.
- 특정 레이어 간의 충돌을 허용하거나 방지할 수 있습니다.


---

## Cloth Inter-Collision (옷 간 충돌) - Bool Type
![](https://i.imgur.com/Jn0lwti.jpg)
- 옷끼리의 충돌 처리를 할 것인지의 여부를 설정합니다.


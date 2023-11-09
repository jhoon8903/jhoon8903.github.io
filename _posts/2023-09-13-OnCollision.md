---
layout: post
title: OnCollision || OnTrigger
subtitle: Unity에서 충돌을 감지하고 데이터를 반환하는 충돌감지기능
categories: Unity
author: Daniel
tags: 
 - Unity
 - Physic
banner:
 image: https://i.imgur.com/PkyfS9F.png
---
![](https://i.imgur.com/PkyfS9F.png)


OnCollision
==

일반적인 콜라이더를 가진 두 게임 오브젝트가 충돌할 때 자동으로 실행 되는 이벤트 메소드
충돌한 두 콜라이거는 서로 통과하지 못한다.

오브젝트 컴포넌트에는 Rigidbody가 있어야 한다.

별도의 호출 없이 이벤트가 발생되면 자동으로 호출되어 실행된다.

매개변수로는 "Collision" 타입의 매개변수를 받으며, 충돌 정보(오브젝트 Info, position, force 등)를 지님

- 두 물체가 물리적으로 충돌했을 때의 반응을 처리하려는 경우 (예: 볼이 벽에 부딪혀 튕기는 효과).
- 땅에 닿았을 때의 플레이어 반응 등.
---

### OnCollisionEnter (Collision collision) : 충돌한 순간
- 발생 시점 : 오브젝트가 최초 충돌할 때 호출
- 예 : 플레이어 오브젝트가 적과 충돌 하였을 때, 데미지를 주거나 볼이 벽에 부딪혔을 때 반사 처리 등.

### OnCollisionStay (Collision collision) : 충돌하는 동안
- 발생 시점 : 오브젝트가 충돌하는 동안 매 프레임마다 호출
- 예 : 오브젝트끼리 계속 충돌상태에 있는 불 속에 있는 플레이어에게 지속적인 데미지를 주는 등 효과

### OnCollisionExit (Collision collision) : 충돌했다가 분리되는 순간
- 발생 시점 : 오브젝트가 충돌상태에서 벗어났을 때 호출
- 예 : 오브젝트가 영역을 벗어났을 때, 효과를 멈추거나, 추가적인 효과를 주고 싶을 때

---

OnTrigger
==

충돌한 오브젝트 중 하나의 Collider가 "Is Trigger" 로 설정되어 있어야 함
물리적 반응 없이 영역 내에서 상호작용을 처리하기 위해 사용

매개변수로는 'Collider' 타입의 매개변수를 받음

- 물리적인 반응 없이 무언가가 특정 영역에 들어왔을 때의 반응을 처리하려는 경우 
  예: 플레이어가 보물 상자 영역에 들어왔을 때 아이템 획득 등
- 적 영역에 플레이어가 들어갔을 때 적의 반응 등.
---

### OnTriggerEnter (Collider other) : 충돌하는 순간
- 발생 시점 : 오브젝트가 트리거 영역 안으로 들어갔을때 호출
- 예 : 플레이어가 함정 영역안에 들어갔을때 발동

### OnTriggerStay (Collider other) : 충돌하는 동안
- 발생 시점 : 오브젝트가 영역 내에 있는 동안 매 프레임마다 호출
- 예 : 플레이어가 회복존 안에 있는동안 회복량 Up

### OnTriggerExit (Collider other) : 충돌했다가 분리되는 동안
- 발생 시점 : 오브젝트가 트리거 영역을 벗어났을 때 
- 예 : 플레이어가 회복존을 벗어나면 회복 중지

---


## 왜 Collision이 아닌 Collider?

OnTrigger 계열에서 Collider 타입이 들어오는데 Collision이 아닌 이유는 
트리거 충돌시에는 상세한 충돌 정보가 필요 없기 때문

트리거 충돌의 경우 Collision충돌과는 다르게 서로를 밀어내지 않고 통과하기 때문에 
Rigidbody의 반발력 관성, 중력등의 영향을 받지 않기에 충돌받은 오브젝트의 Collider정보를 그대로 받아 처리
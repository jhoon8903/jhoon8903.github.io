---
title: Unity 문법
category: Unity
author: 이정훈
tags:
  - unity
img: https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: Unity 문법
---

![](https://media.tenor.com/Fm8FYgSdTPEAAAAi/rolling-cat.gif)

# 게임 개발하면서 작성하는 순서 없는 문법

## Time.deltaTime
```csharp
transform.position += transform.forward * Time.deltaTime * moveSpeed;
```

지난 프레임이 완료되는 데까지 걸린 시간 차이를 의미 ( f/s )
- 한 프레임을 진행하는데 걸린 시간
- 성능과 무관하게 프레임당 실행되는 횟수를 보장
- 
![](https://i.imgur.com/VPnjqtV.png)

정리하면, 슈퍼 컴퓨터는 1초에 1000프레임이라고 가정하자.
그러면 Time.deltaTime는 1/1000(1000분의 1)이다.
이것을 곱하면  1000 * speed * 1/1000 이므로  = speed만 남는다.

게임용 일반 컴퓨터는 1초에 80프레임이라고 가정하자.
그러면 Time.deltaTime는 1/80(80분의 1)이다.
이것을 곱하면  80 * speed * 1/80 이므로  = speed만 남는다.

10년이 지난 구린 컴퓨터는 1초에 20프레임이라고 가정하자.
그러면 Time.deltaTime는 1/20(20분의 1)이다.
이것을 곱하면  20 * speed * 1/20 이므로  = speed만 남는다.

결론 : Time.deltaTime을 이용하면, PC의 성능과는 무관하게 동등한 조건이 되게 되므로 반드시 필요하다.
[Time.deltaTime의 의미와 사용방법](https://codingmania.tistory.com/172)
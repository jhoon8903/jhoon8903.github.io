---
title: Unity Coroutine
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/PjJXyGU.png
comments_disable: true
meta_description: Unity Coroutine
---
# Coroutine 
## Message Loop / Blocking / Multi thread
- Unity Life Cycle
- 초기화 => 게임로직 => 랜더링 => 정리 => 기타 이벤트 등 의 일련의 과정
- 일반적인 함수를 호출하면 해당 함수 안의 로직을 다 수행해야만 실행이 끝남
- 만약, 수행 로직의 시간이 10초 라면 10초 동안 메시지 루프의 다른 로직의 실행이 불가능 해짐
- 이를 **Blocking**(블로킹) 이라고 함
- 메세지 루프에 의한 블로킹을 해결하기 위해 병렬로 함수를 호출 하는 것이 **Multi Thread**(멀티스레드)
![](https://i.imgur.com/PjJXyGU.png)
```csharp
private void Fade()
{
	for(var f = 1f; f >= 0; f -=0.1f)
	{
		var c = GetComponent<Renderer>().material.color;
		c.a = f;
		GetComponent<Renderer>().material.color = c;
	}
}
```
=> Alpha 값이 순식간에 반복 되어 0으로 변해버림

```csharp
private IEnumerator Fade()
{
	for(var f = 1f; f >= 0; f -=0.1f)
	{
		var c = GetComponent<Renderer>().material.color;
		c.a = f;
		GetComponent<Renderer>().material.color = c;
		yield return null;
	}
}
```
=> yield 키워드를 만나면, 제어 권한을 유니티의 메인 Message Loop로 보냄

![](https://i.imgur.com/dwbP633.jpg)

#### yield return null
- '다음 프레임까지 해당 코루틴을 잠시 대기(정지)'하는 동안 Message Loop로 제어권을 넘겨 다른 작업을 처리한다.
- Process가 Blocking 되지 않고 `'멀티 쓰레드'처럼` 비슷한 효과

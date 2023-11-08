---
layout: post
title: Unity [SerializeField]
subtitle: SerializeField를 사용해야 하는 이유와 방법
categories: Unity
author: Daniel
tags: 
 - Unity
 - Script
banner:
 image: https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
---

### `[SerializeField]`

##### Serialize는 직렬화 작업
직렬화 : 추상적 데이터를 전송가능한 형태로 바꾸는 것
역직렬화 : 추상적 데이터를 재조립 하는것 
- Binary Data Save

***

##### SerializeField
- Inspector 에서 접근은 가능하지만, 외부 스크립트에서 접근이 불가능하게 하기 위한 방법
- private 변수를 inspector에서 접근 가능하게 해주는 기능
- 일반적으로 Inspector에서 자주 변경해야 하는 private 변수에 쓰인다.

|c# 스크립트|Inspector View|
|:----:|:---:|
|![](https://i.imgur.com/gLYFJXJ.png)|![](https://i.imgur.com/ncX0IJZ.png)|

## public 접근제한자와 다른점

### `public`

- C#의 일부이며 클래스 밖의 다른 클래스 메소드에 접근할 수 있도록 공개적으로 선언 하는 것
- MonoBehaviour나 ScriptableObject에서 필드를 `public`으로 선언하면 Unity가 그것을 직렬화하고 인스펙터에 표시
- 인스펙터에서만 편집 가능해야 하는 필드를 `public`으로 선언하는 것은 객체 지향 프로그래밍에서 권장되지 않음
- 필드가 코드의 어느 부분에서든 변경될 수 있게 되어 설계 문제를 일으키고 코드 유지 관리를 어렵게 만들기 때문

### `[SerializeField]`

- Unity 특정 속성으로, private 필드 선언 위에 위치시킬 수 있음
- Unity에게 private 필드를 직렬화하고 인스펙터에서 노출하도록 지시하면서도 다른 클래스에서는 접근할 수 없게 함
- 인스펙터에서 값 설정은 가능하지만 여전히 클래스 내부에서만 접근할 수 있는 private 필드의 캡슐화 이점을 유지
- 인스펙터에 변수를 노출하고 싶지만 다른 클래스에서 접근할 필요가 없을 때 권장되는 방법

```csharp
public class Example : MonoBehaviour
{
    public int myPublicInt; // 어디서든 접근 가능하고 인스펙터에서 보임.

    [SerializeField]
    private int myPrivateInt; // 이 클래스 내에서만 접근 가능하지만 여전히 인스펙터에서 보임.
}
```
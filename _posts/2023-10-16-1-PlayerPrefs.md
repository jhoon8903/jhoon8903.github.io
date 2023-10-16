---
title: Unity PlayerPrefs
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/4zFKGVg.jpg
comments_disable: true
meta_description: Unity PlayerPrefs
---
# PlayerPrefs
- 유니티 기본 저장 기능
- 저장 가능 변수 int, float, string, bool

|메서드|기능|
|:--|:--|
|DeleteAll()|모든 키값 삭제|
|DeleteKey(string keyValue)|특정 키값 삭제|
|GetFloat(string key, float default)|지정한 float 타입의 키 값을 로드 (없으면 default 반환)|
|GetInt(string key, int default)|지정한 int 타입의 키 값을 로드 (없으면 default 반환)|
|GetString(string key, string default)|지정한 string 타입의 키 값을 로드 (없으면 default 반환)|
|HasKey(string key)|해당 키가 존재하는지 반환|
|Save()|변경된 모든 키 값을 물리적인 공간에 저장|
|SetFloat(string key, float value)|지정한 키로 float 값을 저장|
|SetInt(string key, int value)|지정한 키로 int 값을 저장|
|SetString(string key, string value)|지정한 키로 string 값을 저장|

```csharp
var damage = PlayerPrefs.GetFloat("DAMAGE");
var playerLevel = PlayerPrefs.GetInt("PLAYER_LV", 1);
```

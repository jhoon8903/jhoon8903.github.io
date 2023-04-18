---
title: Unity [SerializeField]
category: unity
author: "이정훈"
tags: [unity]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity [SerializeField]"
---

### `[SerializeField]`

##### Serialize는 직렬화 작업
직렬화 : 추상적 데이터를 전송가능한 형태로 바꾸는 것
역직렬화 : 추상적 데이터를 재조립 하는것 
- Binary Data Save

***

##### SerializeField
-  Inspector 에서 접근은 가능하지만, 외부 스크립트에서 접근이 불가능하게 하기 위한 방법
- private 변수를 inspector에서 접근 가능하게 해주는 기능
- 일반적으로 Inspcetor에서 자주 변경해야 하는 private 변수에 쓰인다.

|c# 스크립트|Inspector View|
|:----:|:---:|
|![](https://i.imgur.com/gLYFJXJ.png)|![](https://i.imgur.com/ncX0IJZ.png)|
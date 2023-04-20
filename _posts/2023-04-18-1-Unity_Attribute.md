---
title: Unity Attribute
category: unity
author: "이정훈"
tags: [unity, c#]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity Attribute"
---

|Attribute|Notice|
|:---:|:----|
|SerializeField|private, pritected 변수를 Inspector View에 노출|
|Serializable|Custom Class를 Inspector View 노출|
|Header|변수위에 타이틀을 설정해 카테고리 분류 가능<br>Inspector View 정리|
|HideInInspector|public 변수를 Inspector에서 숨길 수 있음|
|RequireComponent|필수 컴포넌트를 추가 할 수 있음|
|Range|Int, float 변수를 슬라이드바로 표시하고 범위를 제한|
|Space|변수와 변수사이에 간격|
|CreateAssetMenu|ScriptableObject Asset을 생성할때 사용하는 메뉴를 추가 할 때 사용|
|MenuItem|임의의 함수 ( static ) 실행을 메뉴 항목으로 추가|
|ContextMenu|임의의 함수 ( non-static ) 실행을 컴포넌트 톱니 메뉴에 추가|
|AddComponentMenu|인스펙터의 AddComponent 메뉴 항목으로 컴포넌트를 추가 할때 사용|
|ExecuteInEditMode|에디터가 플레이 모드가 아니더라도 컴포넌트가 동작하도록 할때 사용|
|Multiline|string 변수를 여러줄 입력 가능하게 만들때 사용|
|TextArea|Multiline과 비슷, 폭에 맞춰 자동으로 줄바꿈과 슬라이드바 표시|
|Tooltip|인스펙터에 표시되는 변수에 설명을 추가 할때 사용|

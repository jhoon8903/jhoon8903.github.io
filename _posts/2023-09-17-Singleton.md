---
title: Unity Singleton
category: Unity
author: 이정훈
tags:
  - unity
img: 
comments_disable: true
meta_description: ""
---

# Singleton 

- 게임 매니저처럼 관리자 역할을 하는 오브젝트는 일반적으로 프로그램에 단 하나만 존재해야함
- 언제 어디서든 즉시 접근 가능해야함

### 단일 오브젝트
- 점수, UI, 게임 상태를 관리하는 게임 매니저는 게임에 단 하나만 존재해야 함

### 손쉬운 접근
- 어느 곳에서도 쉽게 접근할 수 있도록 구현 해야 함

### 싱글턴을 사용하는 이유
- 게임 매니저 오브젝트는 단 하나만 존재
- 어떤 곳에서고 손쉽게 게임 매니저 오브젝트에 접근 가능

---

# Static

- 싱글턴 패턴을 구현 시 정적(static) 변수의 특징을 활용
- 메모리에 단 하나만 존재하고 모든 오브젝트가 공유
- Class 이름과 `.` 연산자를 이용하여 접근
![](https://i.imgur.com/CT5B3lM.png)
![](https://i.imgur.com/vAnJH6G.png)

---
layout: post
title: 팀 프로젝트 소개
subtitle: 팀 프로젝트 발표
author: Daniel
categories: Project
tags: 
 - Project
banner:
  image: https://i.imgur.com/KsvqiIh.jpg
---

프로젝트 PT
--

팀 프로젝트는 중점을 게임 개발 구현과 아트적 도전 및 구현이 아닌     
객체 지향과 GIt 사용에 중점을 두고 개발을 진행 함

### 프로젝트 소개

- 프로젝트는 3개의 OS 에 대한 호환 및 확장을 해결하는 것이 문제였고
- 깃을 처음 사용하는 팀원들이 깃 이용에 능숙해지는 것이 목표

![](https://i.imgur.com/ZuGtOs6.jpg)

- 깃은 각 개개인별 기능에 대한 브랜치를 별도로 생성하고, 컨벤션을 만들어 지키는 것을 연습

![](https://i.imgur.com/0hTAcb6.jpg)

- 각 기능은 Scene 별로 구분하고 각 객체들을 Class로 분류하여 객체지향 디자인을 가짐

### 프레임워크

![](https://i.imgur.com/RHRYMOW.jpg)

- 추상 클래스를 생성하여 각 씬은 상속을 받아 작성하며
- 각 OS 별 API 를 조건을 확인하여, API를 구분하여 각 OS 맞도록 개발 구현
- 자주 사용되는 Renderer이 경우 별도의 클래스로 생성하여 재사용성을 높힘

### 디자인

![](https://i.imgur.com/KsvqiIh.jpg)

- 팀 협업에서 Conflict를 방지하기 위해 종속성을 분리하여 원할한 협업을 위한 디자인을 설계
- 각 씬은 서로간의 종속성을 위해 상호 액세스 하지 않고, Manager를 통해 통신
- 필요한 소스 및 유틸은 각 소스 폴더에서 참조하여 가져오는 디자인을 적용
- 빌드는 각 OS 별 API에 맞춰 빌드를 진행

### 프로젝트 트러블

- 대부분 팀원들은 깃을 사용하는 것에 많은 어려움을 겪으것으로 보임

![](https://i.imgur.com/6cUgpOD.jpg)

- 처음 개발을 접한 팀원들의 경우 각 OS 호환성을 해결하는데 어려움을 겪음

![](https://i.imgur.com/FuMCa9N.jpg)

- 특히 Git의 경우는 최신 브랜치에 업데이트 및 의사소통의 중요성을 알게 됨

![](https://i.imgur.com/amdFq8f.jpg)

![](https://i.imgur.com/KTN1paB.jpg)

- 결국 애자일 하게 작게 작게 개발하고 계속 의사소통을 하여 서로간의 충돌을 방지하는 것이 중요함

마치며
--

- 커뮤니케이션과 세팅이 매우 중요하다는 것을 다시 한번 각인하는 좋은 기회가 되었습니다.
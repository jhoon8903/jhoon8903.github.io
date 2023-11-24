---
layout: post
title: Cinemachine으로 케릭터 Follow 하기
subtitle: Cinemachine Asset 으로 케릭터 이동 추적하기
author: Daniel
categories: Unity
tags: 
 - Project
 - Camera
 - Cinemachine
 - Unity
banner:
  image: https://i.imgur.com/qTnotkZ.gif
---

Cinemachine
--

2020 버전 이후 나온 Cinemachine 은 가상의 카메라를 설정하여 사용자가 보여주고 싶은 화면을 자연스럽게 또는 멋지게 연출할 수 있는 기능으로 다양한 영상 연출에 사용되는 에셋입니다.

여기서는 별도의 코딩 없이 가상카메라가 플레이어를 따라다니는 Follow Cam을 만들예정입니다.

### 1. Asset 설치

- Package Manager에서 Unity Registry 카테고리에서 `Cinemachine`을 설치

![](https://i.imgur.com/Of956n8.jpg)

### 2. Virtual Camera 생성

- 하이어라키상에서 => Cinemachine => Virtual Camera를 생성합니다.

![](https://i.imgur.com/V4r0nHs.jpg)

### 3. Virtual Camera Inspector

- 정말 다양한 기능이 있지만 여기선 목적에 맞는 카메라 Follow  기능만을 소개합니다.

![](https://i.imgur.com/InFyNae.jpg)

#### Follow
- 카메라가 Follow 할 오브젝트를 할당하면 해당 오브젝트를 카메라가 자동으로 따라갑니다.

#### Loot At
- 카메라가 Game 화면에 보여줄 오브젝트를 할당하면 해당 화면을 비춥니다.

#### lens Vertical FOV
- 화각을 설정하면 숫자가 작으면 좁은 각을 커지면 커질수록 넓은 화면을 보여줍니다.

#### Body
- 카메라의 설정이며, 다양한 기능을 제공하지만 여기서는 플랫한 카메라 무빙을 보여주는 `Transposer`을 사용합니다.

#### Aim
- 카메라의 촛점 즉 어디를 중앙으로 비추는지를 선택하며 `Hard Look At`으로 하면 Follow하는 오브젝트를 계속해서 고정되어 따라갑니다.

- 추가적인 정보는 Unity 공식 [Docs](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.2/manual/CinemachineVirtualCamera.html)를 확인하시면 됩니다.

> 이렇게 간단하게 세팅을 하게 되면 별도의 코딩 없이 카메라는 항상 케릭터를 Follow 하게 됩니다.


![](https://i.imgur.com/qTnotkZ.gif)

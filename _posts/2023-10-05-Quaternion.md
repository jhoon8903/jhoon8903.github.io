---
title: Unity Quaternion
category: Unity
author: 이정훈
tags:
  - unity
img: 
comments_disable: true
meta_description: Unity Quaternion
---
# Quaternion (쿼터니언)
- Unity 에서 사용되는 각도의 단위
- 복소수 4차원 벡터 (four-dimensional complex number)
- 4개의 원소로 표현 (x, y, z, w)

🔆 일반적인 오일러 각 (Euler Angle)
![](https://i.imgur.com/ccdpoEW.jpg)

🔆 Unity 에서 사용되는 쿼터니언 타입
![](https://i.imgur.com/vh4W3ZT.jpg)

## Euler Angle 의 문제점
- 물체가 회전하는 동안 X, Y, Z 축 중 2개의 축이 겹쳐지면, 어느 축으로도 회전하지 않고 잠기는 현상
- Gimbal Lock 발생
![](https://blog.kakaocdn.net/dn/dlzqNK/btqMVDwWBFS/65yM1KLhiACMTjth5kbSDK/img.gif)

## Quaternion의 해결 방법
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTIk6y%2Fbtsl1zg3lMK%2FwL5QxfyoqwRauCc0wKjM90%2Fimg.gif)
- 세 개의 축을 동시에 회전시켜 짐벌락 현상을 방지
- 짐벌락 방지를 위해 모든 객체의 Rotation을 쿼터니언으로 이용해 처리

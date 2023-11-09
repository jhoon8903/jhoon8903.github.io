---
layout: post
title: Mathf Class
subtitle: C#에서 수학적 계산을 지원하는 Class
category: C#
author: Daniel
tags: 
 - Programming
 - Csharp
 - Language
 - Math
banner:
 image: https://i.imgur.com/thFCOHI.png
---

![](https://i.imgur.com/thFCOHI.png)

Mathf Class 
--
Unity에서 제공하는 다양한 수학 함수

###  상수
---
- `PI`: float - 원주율 값.
- `Infinity`: float - 무한대 값.
- `NegativeInfinity`: float - 음의 무한대 값.
- `Epsilon`: float - 아주 작은 수.

### 기본연산
---
- `Abs(float f) -> float`: 절대값 반환.
- `Sign(float f) -> float`: 값의 부호 반환 (+1, -1 또는 0).
- `Max(float a, float b) -> float`: 두 값 중 최대값 반환.
- `Min(float a, float b) -> float`: 두 값 중 최소값 반환.
- `Pow(float f, float p) -> float`: 거듭제곱 값 반환.
- `Sqrt(float f) -> float`: 제곱근 값 반환.

### 반올림 및 정수 연산
---
- `Ceil(float f) -> int`: 올림값 반환.
- `CeilToInt(float f) -> int`: 올림값을 정수로 반환.
- `Floor(float f) -> int`: 내림값 반환.
- `FloorToInt(float f) -> int`: 내림값을 정수로 반환.
- `Round(float f) -> int`: 반올림 값 반환.
- `RoundToInt(float f) -> int`: 반올림 값을 정수로 반환.

### 삼각함수
---
- `Sin(float f) -> float`: 사인 값 반환.
- `Cos(float f) -> float`: 코사인 값 반환.
- `Tan(float f) -> float`: 탄젠트 값 반환.
- `Asin(float f) -> float`: 아크사인 값을 반환.
- `Acos(float f) -> float`: 아크코사인 값을 반환.
- `Atan(float f) -> float`: 아크탄젠트 값을 반환.
- `Atan2(float y, float x) -> float`: 두 숫자의 아크탄젠트 값을 반환.

##### 아크사인 (Arcsine 또는 Inverse Sine)

아크 사인은 사인 함수의 역함수로, 주어진 값의 사인 값이 얼마인지 알려주는 각도를 찾아주는 함수입니다. 사인 함수가 각도를 입력으로 받아 그 각도의 사인 값을 출력하는 함수라면, 아크 사인은 사인 값을 입력으로 받아 그 값을 갖는 각도를 출력합니다.

아크 사인은 주로 (−1,1) 범위의 값에 대해 (0, π) 범위의 결과를 반환합니다. 
예시:
- asin(0)= 0asin(0)=0 (0도)
- asin(1)= 2asin(1)=2π​ (90도)
- asin(−1)= −2asin(−1)=−2π​ (-90도)


### 보간 및 제한
---
- `Clamp(float value, float min, float max) -> float`: 값을 범위 내로 제한.
- `Clamp01(float value) -> float`: 0과 1 사이로 값을 제한.
- `Lerp(float a, float b, float t) -> float`: 두 값 사이를 선형 보간.
- `LerpUnclamped(float a, float b, float t) -> float`: 제한 없이 두 값 사이를 선형 보간.
- `InverseLerp(float a, float b, float value) -> float`: 선형 보간의 역 연산.
- `MoveTowards(float current, float target, float maxDelta) -> float`: 
  현재 값에서 목표 값으로 지정된 양만큼 이동.

##### 보간 (Interpolation)

보간은 두 값 사이의 중간 값을 추정하는 과정을 의미합니다. 예를 들어, 두 점 A와 B 사이의 중간 값을 찾고자 할 때, 보간을 사용하여 그 값을 추정할 수 있습니다.

**선형 보간 (Linear Interpolation, Lerp)는 가장 간단한 보간 방법 중 하나로, 두 값 사이의 일정한 비율에 따라 중간 값을 계산합니다. 

수식으로는: Lerp(A,B,t)=A+t×(B−A)

여기서 t는 보간 비율을 나타내며, 0과 1 사이의 값을 가집니다. t=0일 때 결과는 A이며, 
t=1일 때 결과는 B입니다. t의 값이 0과 1 사이일 때는 A와 B 사이의 중간 값을 얻게 됩니다.

Unity의 `Mathf` 클래스에서는 `Lerp` 메서드를 통해 선형 보간 값을 계산할 수 있습니다.

### 로그 및 지수 함수
---
- `Exp(float power) -> float`: e의 지정된 거듭제곱을 반환.
- `Log(float f) -> float`: 자연 로그 값을 반환.
- `Log(float f, float p) -> float`: 지정된 밑수의 로그 값을 반환.

### 기타 연산
---
- `DeltaAngle(float current, float target) -> float`: 두 각도 사이의 최소 차이 반환.
- `PingPong(float t, float length) -> float`: 특정 길이의 범위 내에서 왔다갔다하는 값 반환.
- `Repeat(float t, float length) -> float`: 값이 지정된 길이를 초과하면 0에서 다시 시작.
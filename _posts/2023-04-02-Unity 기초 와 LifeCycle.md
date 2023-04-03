---
title: Unity LifeCycle
category: Unity
author: "이정훈"
tags: [Unity]
img : https://img.etnews.com/photonews/2103/1396211_20210325190939_408_0012.jpg
comments_disable: true
meta_description: "Unity LifeCycle"
---

# 게임은 하나의 연극이다.

![](https://cdn.gaminggorilla.com/wp-content/uploads/2023/02/The-Most-Popular-Games-to-Play-Right-Now.jpg)

[이미지 출처-Gaming Gorilla](https://gaminggorilla.com/most-popular-video-games-now/)

|연극|막|인물, 소품|대본|
|:---:|:---:|:---:|:---:|
|게임|장면|오브젝트|스크립트|

***

# 언어 'C #' 을 사용

## Data Type

### 자료형
##### int : 정수형 데이터
```c#
int level = 10;
```

##### float : 숫자형 데이터 (끝에 f를 붙여야 함)
```c#
float strength = 15.5f;
```

##### string : 문자열 데이터
```c#
string playerName = 'Hero'
```

##### bool : 논리형 데이터
```c#
bool servival = true;
```

### 그룹형 (array, List)

#### 배열형
```c#
string[] monster = {'슬라임', '늑대', '거미'};

// data 추가
int[] monsterLevel = new int[3];
monsterLevel[0] = 1;
monsterLevel[1] = 10;
monsterLever[2] = 11;

Debug.Log(monsterLevel[0]) // 1
Debug.Log(monsterLevel[1]) // 10
Debug.Log(monsterLevel[2]) // 11
```

#### 리스트형
```c#
List<datatypes> items = new List<datatypes>();

// data 추가
List<string> items = new List<string>();
items.Add('생명포션');
items.Add('마나포션');
items.Add('해독포션');

Debug.Log(items[0]); // 생명포션
Debug.Log(items[1]); // 마나포션
Debug.Log(items[2]); // 해독포션

// data 삭제
items.RemoveAt(0);

Debug.Log(items[0]); // Range Exception Error
Debug.Log(items[1]); // 마나포션
Debug.Log(items[2]); // 해독포션
```

***
# Unity Life Cycle

![](https://i.imgur.com/5bkYLPT.png)

## 초기화
### - Awake(  ) : 게임 오브젝트 생성할 때, 최초 실행
### - Start(  ): 업데이트  시작 직전, 최초 실행

## 활성화
### - OnEnable(  ) : 게임 오브젝트가 활성화 되었을 때

## 물리 (프레임 단위로 실행)
### - FixedUpdate(  ) : 물리연산 업데이트
- 고정된 실행 주기로 **CPU를 많이 사용함**
- 물리연산과 관련된 함수만 넣음
- 1초에 약 50 ~ 60회 실행된다 (프레임 수)

## 게임로직
### - Update(  ) : 게임 로직 업데이트
- 환경에 따라 **실행주기가 떨어질 수 있음** (FixedUpdate에 비해서)
### - LateUpdate(  ) : 모든 업데이트를 끝낸 후 (후 처리)

## 비활성화
### - OnDisable(  ) : 게임 오브젝트가 비활성화 될 때

## 해체
### - OnDestroy(  ) : 게임 오브젝트가 삭제 될 때

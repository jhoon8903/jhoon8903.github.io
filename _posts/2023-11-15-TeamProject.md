---
layout: post
title: 팀 프로젝트 기획
subtitle: HexCore Vilige 기획
author: Daniel
categories: Project
tags: 
 - Project
 - Game
 - Programming
 - Csharp
 - 게임 기획
banner:
  image:
---

HexCore Village
--

|:--|:--|
|기간|2023.11.15 ~ 2023.11.22 (7일)|
|장르|C# Console Text Game|
|개발인원|6 명|
|개발환경|Window, Mac|
|IDE|Visual Studio, Rider|

### 팀 프로젝트 기획
#### 프로젝트 목적

- 콘솔 개인 프로젝트에 이어서 던전에서의 전투 시스템 구현
- Git 을 이용한 협업 경험 향상

#### 프로젝트 컨벤션

**Git Hub**

- 🔖 양식 [적용내용] - 적용된 항목 Title
	- 어떤 문제가 발생 
	- 해당 부분은 어디서 발생한 문제 
	- 해당 부분을 어떻게 수정 (코드는 필요 없습니다.) 
	- 자세한 내용은 주석을 이용합니다. 

- 🧾 커밋메세지 (간단하게 3개만) 
	- [💫 Feature] : 신규 기능 개발 
	- [🐛 BugFix] : 발생한 버그에 대한 해결 
	- [🛠️ Fix] : 기능에 대한 수정 

- 🔯 Git Branch - Git Branch 생성은 [ 이름이니셜 - 기능 ] 양식으로 작성 
	- 해당 브랜치는 Main 브랜치가 아닌 Develop 브랜치에 커밋 
	- 커밋 이후에는 브랜치는 삭제하고 새로운 브랜치 생성 
	- 커밋은 꼭 기능이 끝나면 자주 커밋하여 팀원들의 프로젝트가 최신을 유지 
	- 커밋시 "어떤 Scene 커밋 하겠습니다" 라고 이야기해주세요 
	- Conflict가 나면 혼자 해결 하시기 보다는 도움을 요청하세요

**Code**

- 🏞️ 레이아웃 규칙 
	- Console.WriteLine("한 줄에 하나씩"); 
	- string variable = "변수 선언은 한줄에"; 
	- 메서드간 정의 및 속성은 빈 한줄로 잘라서 표현 
```csharp
public int[] number = {2,4,5,6,7};

if (number % 2 == 0) 
{ 
	Consol.WriteLine(number); 
}
```
		
- 🖋️ 주석달기 
	- class, Method 상단에는 /// 를 이용한 Summary 작성 
	- 해당 메서드 또는 클래스가 담당하고 있는 기능 
	- 입력 값이 있다면 입력 값에 대한 내용 Input (Character character.Name) 
	- 반환 값이 있다면 반환값 return (string 반환값, int 반환값) 
	- 다른 팀원과 공유하는 자원의 경우 // 를 이용한 주석 
	- // 주석은 한칸 띄어서 사용합니다. 

- 🖊️ 문자열 보간 
	- 문자열에 변수를 담아낼때는 $"{문자열보간}을 사용합니다."; 
	 
- 🔍 변수 선언 
	- 변수의 타입이 분명하면 해당 타입을 
	- 불분명 하다면 var 선언 (반환타입이 확인이 된다면 바꿔주시기 바랍니다.) 
	- 변수는 길더라도 명확하게 이해 할 수 있는 변수명을 사용해주시기 바랍니다.

### 프로젝트 소개

- 내일배움개발 1조 HexaCore의 플레이어 A씨 게임 개발 도중 실행 중 버그가 발생했다.
- 발생한 버그를 해치우며 레벨업 하여 게임을 완성하자!

#### Login

![](https://i.imgur.com/bJ8efC3.jpg)

- Json을 이용한 Character Data Parsing
- 최초 케릭터 생성시 기본 Json 데이터를 생성
- ID 만으로 조회하여 게임 로드를 가능하도록 데이터 관리
- 아이디 입력 => 직업 선택 => 직업 보너스 배경 선택 등의 컨셉을 잡고 개발 진행

#### GameManager

![](https://i.imgur.com/8I0I3wX.jpg)

- Console ReadKey 입력은 예외처리에 대한 부분과 시각적인 부분에서 Arrow Function 키로 작업
- 정적 데이터 및 구조체 클래스 등을 이용한 클래스간의 의존서을 낮춰 협업시 문제가 없도록 설계
- Manager의 싱글톤으로 게임을 컨트롤

#### Character 

![](https://i.imgur.com/euolLwR.png)

- ASC를 이용한 Stauts UI 표현 시도
- 정적파일과 관리되는 아이템의 데이터를 로드하여 직렬화 및 역직렬화를 통해서 Json 객체 관리
- 추가적인 사항들은 이후 프로젝트 진행도에 따라서 추가될 예정

#### Store And Inventory

![](https://i.imgur.com/cLji40O.png)


- Json 객체를 이용한 Data 처리
- 아이템 리스트의 경우 Arrow Key 를 이용해서 스크롤 기능 구현
- 싱글톤 으로 데이터 관리
- 아이템 클래스의 경우 Dictionary 형식의 데이터를 ItemName을 Key로 Access

#### Battle

![](https://i.imgur.com/Q8CyQkI.png)

- 턴제 방식의 배틀 시스템 구현
- 턴 진행시 UI에 log 식의 데이터 처리

#### Reward

![](https://i.imgur.com/tqzeCIl.png)

- 전투 결과를 Bool 값으로 가져와 각 해당 조건에 따라서 보상진행
- 기본 시스템 개발 후 기타 Reward Item의 경우 개발시 추가할 예정

### Utility

#### Dummy Data
- 개발시 모든 품목 및 상태를 정할 수 없기 때문에 기초적인 Dummy Data 작성
- 케릭터 및 아이템의 기본 데이터는 Json 파일로 생성

![](https://i.imgur.com/WWvwKCg.png)

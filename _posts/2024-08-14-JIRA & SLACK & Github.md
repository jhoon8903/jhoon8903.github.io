---
layout: post
title: JIRA & SLACK & Github 연동
subtitle: Orgazination Company
author: Daniel
categories: Third Party
tags: 
banner:
  image:
---

# 1. 프로젝트 생성

- "Create Project" 를 선택하여 프로젝트 생성

![](https://i.imgur.com/3RBdU3o.jpeg)

![](https://i.imgur.com/zoERPVz.jpeg)

- "Software development" => "Scrum" 선택하여 프로젝트 생성

![](https://i.imgur.com/qAMJc9U.jpeg)

-  Use Template => "Select a Team-managed project"

![](https://i.imgur.com/UBO2PSZ.jpeg)

- Name : 프로젝트 명
- Access : Limited
- Key : 자동으로 설정 되나 보통 "프로젝트의 약자"
	- ex. "Convenience Sort" => "CS"
![](https://i.imgur.com/GeahqrY.jpeg)

# 2. Third Party Tool 연결 
![](https://i.imgur.com/aWtILeR.jpeg)

- Github : "Connect Repository" 선택하여 프로젝트 Repo 연결
- Confluence : "해당 프로젝트 페이지 연결" (옵션사양)

# 3. 프로젝트 세팅

- "Project Setting" 
	- Name : 프로젝트 Name
	- Key : 프로젝트 Unique Key
	- Project Lead : 프로젝트 담당자
	- Default Assignee : 초대 된 다른 사용자의 기본 권한
		- Project Lead : 프로젝트 리더
		- Unassigned : 할당하지 않음 *

| 프로젝트세팅                                | 디테일                                   | Access                                |
| ------------------------------------- | ------------------------------------- | ------------------------------------- |
| ![](https://i.imgur.com/3xvySY3.jpeg) | ![](https://i.imgur.com/yerHkq7.jpeg) | ![](https://i.imgur.com/5oCQPmU.jpeg) |
|                                       |                                       |                                       |

- "Add People" => 해당 프로젝트 맴버 E-mail 추가 (여러계정 복수 초대 가능)
- "Role" => 초대하는 맴버의 권한 설정 
	- Administrator : 프로젝트 관리자 (설정 변경의 권한)
	- Member : 기본 사용자 (기본적인 Read / Write)
	- Viewer : Only Read

|                기본 초대 폼                |                 초대 시                  |
| :-----------------------------------: | :-----------------------------------: |
| ![](https://i.imgur.com/UQVd1Un.jpeg) | ![](https://i.imgur.com/0okscML.jpeg) |

![](https://i.imgur.com/PzDgWQN.jpeg)

# 3. Slack 연결
- "Add new connection"
	- Workspace : Slack 연결
	- Channel : 해당 슬랙 채널 연결 
		- ex. 편의점 정리왕 => # dev02-편의점정리왕
- Connect Project
![](https://i.imgur.com/Q9d0vDU.jpeg)

![](https://i.imgur.com/sDK73gS.jpeg)

![](https://i.imgur.com/QXq5hpS.jpeg)

# 4. Github 연동

## Github 사용자 설정
- Github Profile => Setting => "Public email" => "본인 계정 선택"

|            Git User Option            |             User Setting              |
| :-----------------------------------: | :-----------------------------------: |
| ![](https://i.imgur.com/krYuHTl.jpeg) | ![](https://i.imgur.com/B3uCG7L.jpeg) |

## Repo 관리자
- Slack Channel 입장
- 채널 채팅에 해당 커맨드 입력 
	- /github subscribe ActionFitGames/프로젝트 레포지토리 이름
	- ex. 편의점 정리왕 : /github subscribe ActionFitGames/Convenience_Sort


![](https://i.imgur.com/sPZExDo.jpeg)

![](https://i.imgur.com/m4MMTpf.jpeg)

- 처음 해당 Repository를 구독하면 알림이 실행되는 옵션
	- Issues
	- pulls
	- commits
	- releases
	- deployments

- 필요없는 기능에 대한 구독 해제
	- /github unsubscribe ActionFitGames/프로젝트 레포지토리 이름 명령어
	- ex. 편의점 정리왕 프로젝트의 "commit"알림 구독 해제
		- /github unsubscribe ActionFitGames/Convenience_Sort commits

- Unsubscribe Options
	- commits
	- releases
	- deployments

![](https://i.imgur.com/bOP2Hja.jpeg)


# Sprint 설정

- Jira Project 설정

![](https://i.imgur.com/VvFWwz6.jpeg)

- "Edit sprint" 선택

![](https://i.imgur.com/l0uNgxg.jpeg)

- Sprint Setting 
	- Sprint name : 스프린트의 이름
	- Duration : 스프린트 기간
		- 스프린트 기간은 다음 업데이트 기준으로 설정 ("Custom")
		- 스테이지 업데이트의 경우에도 중간에 이런저런 버그 및 수정 사항이 있기때문
	- Start Date : 해당 업무를 받은 날짜
	- End Date : 해당 업무 마감 날짜 
	- Sprint Goal : 해당 스프린트가 달성 되었을 때 기대할 수 있는 변경사항

![](https://i.imgur.com/nZiF3Vm.jpeg)

- Sprint 적용 후
![](https://i.imgur.com/sHwu5lL.jpeg)

- Issue 생성
	- Jira의 경우 "Issue" 단위로 프로젝트를 관리
	- Issue 생성시 "{ProjectKey}-{Issue Count}" 단위로 이슈 번호 생성 
		  Issue Count의 경우 자동으로 순서대로 할 당 하며 해당 이슈를 삭제하여도 다음 순서로 넘어감
	- Sprint 생성 후 최소 1개 이상의 Issue 가 있어야 Sprint 를 실행 할 수 있음
	
![](https://i.imgur.com/Zz8Z57R.jpeg)
\
- Issue 생성 후 Board Tab을 확인하여 프로젝트 이슈 확인
	- 최초 Issue 생성 후 "+ Create Issue" 로 Issue 생성 가능
![](https://i.imgur.com/7Ozlibw.jpeg)

- Issue 가 생성되면 앞에서 연결 한 Slack에 해당 Issue 알림이 도착
![](https://i.imgur.com/ZqRdRtH.jpeg)
## Issue 관리

- Issue를 선택하여 해당 Issue에 관한 설명 및 기타 설정 기능을 관리

![](https://i.imgur.com/ruajWyR.jpeg)

|              Issue View               | Issue DESC                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| :-----------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![](https://i.imgur.com/PBQ9U05.jpeg) | - Assignee (Require): Issue 담당자<br>• 해당 Issue를 관리하는 담당자<br><br>- Label (Optional) : Issue Label<br><br>- Parent (Optional)<br>• Hierarchy상에 부모 Issue<br><br>- Team (Optional) <br>• 협업 시 사용할 수 있는 Team Tag<br><br>- Sprint : 해당 Sprint<br><br>- Story point estimate (Require)<br>• 해당 이슈의 복잡도 (난이도)<br>• 해당 Issue의 작업 난이도 (1 => 5)<br>1. 매우 쉬움 (1시간 이내) <br>   ‣ 간단한 데이터 교체, Assets 교체 작업 등<br>2. 쉬움 (2시간 이내)<br>   ‣ 비교적 간단한 버그 수정 등<br>3. 보통 (5시간 이내)<br>   ‣ 일반적인 신규 기능 또는 UI 작업 등<br>4. 어려움 (5시간 이상)<br>   ‣ 프로젝트 일부에 영향을 끼치는 버그 수정 및 신규 기능 등<br>5. 매우 어려움 (1일 이상)<br>   ‣ 프로젝트 전반에 영향을 끼치는 버그 수정 및 신규 기능 등<br><br>- Development (Require)*<br>• Create Branch : Git에 해당 Branch 생성<br>• Create Commit : Git에 해당 Commit<br><br>- Reporter (Require) : <br>• 해당 Issue의 감독자 혹은 확인 담당자 (Supervisor) |

### Create Branch

![](https://i.imgur.com/XdOGrOr.jpeg)

- Branch name : {Project Key}-{issue Number}-{Bracn Name}
	🔴 "Project Key" , "Issue Number" 는 수정 하면 안됩니다.
		해당 Key를 가지고 Brach를 구분하는 System

- 해당 Branch가 Git에 생성 됨
![](https://i.imgur.com/vCQbTHn.jpeg)

### Commit 
- Create Commit 시에는 "Issue Key"를 사용하여 Commit 진행

![[Screenshot 2024-08-18 at 5.10.55 PM.jpg]]
- Terminal 및 Desktop 에서 커밋 시

![](https://i.imgur.com/QXdlygn.jpeg)

![](https://i.imgur.com/yWRDNvZ.jpeg)

# Issue Progress

- Issue를 커밋하고 나면 해당 버그 테스트를 위해 QA 또는 만들어 둔 Progress로 해당 Issue Tab으로 이동
  
![](https://i.imgur.com/kEg2T2t.jpeg)

- QA에서 이상이 있다면 QA 팀은 해당 이슈에 "Add" 로 영상 및 Image 업로드 가능
-  Desc를 작성하여 개발자에게 알림
![](https://i.imgur.com/QdCjA6s.jpeg)

- Debug Tab으로 이동
![](https://i.imgur.com/a1Rf8P4.jpeg)

- 문제가 없다면 "DONE" 탭으로 이동하여 해당 Issue를 종료 합니다.

![](https://i.imgur.com/ysluRZU.jpeg)


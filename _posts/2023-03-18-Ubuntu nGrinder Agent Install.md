---
layout: post
title: Ubuntu nGrinder Agent Install
subtitle: 네이버 클라우드에 테스트를 위한 nGrinder 설치
categories : Project
author: Daniel
tags: 
 - Tooltip
 - Test
 - Cloud
 - Project
banner:
 image : assets/images/posts/kRmfd56.png
---

Instance Create
--

![](https://i.imgur.com/kRmfd56.png)

# Public IP Create

![](https://i.imgur.com/0sUWfb7.png)

![](https://i.imgur.com/laSOuZr.png)

# ssh Access

![](https://i.imgur.com/7VntxMq.png)

## 인증키 (.pem) 으로 관리자 비밀번호 확인
![](https://i.imgur.com/g2Qmvmo.png)

![](https://i.imgur.com/tGrQ4Wg.png)

## ssh 접속
```
$ ssh -i [path/key] root@public_IP
```

```
$ ssh -i /Users/daniel/Desktop/Key/Naver_Agent.pem root@101.101.218.92
```

![](https://i.imgur.com/7ivBmP9.png)

## password 입력
- 관리자 비밀번호 입력
![](https://i.imgur.com/8he0LPF.png)

![](https://i.imgur.com/unGttcA.png)

## apt update
```
$ sudo apt update
```

## JAVA Install
```
$ apt install openjdk-11-jre-headless
```

## 관리자 password 변경
```
$ passwd
$ 새로운 비밀번호 입력
```

![](https://i.imgur.com/mEGycDe.png)

## nGrinder Agent 받기 
```
wget http://컨트롤러 설치 주소:8080/agent/download/ngrinder-agent-3.5.8.tar
```

### 컨트롤러 실행되어 있어야 함
```
wget http://175.210.53.35:8080/agent/download/ngrinder-agent-3.5.8.tar
```

## 에이전트 압축 풀기
![](https://i.imgur.com/fJhMP0C.png)

```
$ tar -xvf ngrinder-agent-3.5.8.tar
```

```
$ cd ngrinder-agent-3.5.8
```

## __agent.conf 설정
```
$ vim __agent.conf
```

```
common.start_mode=agent 
agent.controller_host= Controller IP주소 
agent.controller_port=16001 
#agent.subregion= 
#agent.owner=
```

## Agent 권한 주기
```
$ chmod u+x run_agent.sh
$ chmod u+x run_agent_internal.sh
```

![](https://i.imgur.com/gEkKf8I.png)

![](https://i.imgur.com/A45NnaO.png)

## Agent 실행 (다중실행)
```
$ ./run_agent.sh -ah [에이전트홈] -hi [호스트네임]
```

### 1번 에이전트
```
./run_agent.sh -ah ~/.ngrinder1 -hi agent1
```

### 2번 에이전트
```
./run_agent.sh -ah ~/.ngrinder2 -hi agent2
```

### 3번 에이전트
```
./run_agent.sh -ah ~/.ngrinder3 -hi agent3
```

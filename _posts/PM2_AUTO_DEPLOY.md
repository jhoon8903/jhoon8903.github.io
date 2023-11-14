---
layout: post
title: GameServer Deploy
subtitle: PM2 및 Git Action을 이용한 CI/CD
author: Daniel
categories: Network
tags: 
 - Network
 - Github
 - GitAction
 - NodeJS
banner:
  image:
---

Git Action과 pm2를 이용한 무중단 배포
--

### pm2 설치

> PM2<br>PM2 란  어플리케이션을 `Backgroud`에서 지속적으로 실행 시키며, 만약 충돌 발생시 자동으로 재식작 하는 기능으로 많이 사용합니다.<br><br>Production 환경에서 App을 관리하고 유지하기 쉽게 만들어 줍니다.<br>Node 사용자들의 최소한의 Down Time으로 활성화 된 상태를 유지하고, 성능을 향상 시킬 수 있습니다.

A) Instance에서 Node 에 접근하여 `npm`을 이용하여 `PM2`를 설치

```linux
[ec2-user@ip-172-31-38-37 rpg-socket-server]$ npm install pm2 -g
```

- 설치가 완료 되면 pm2로 스크립트를 실행 시킬 수 있다

```linux
$ pm2 start socketServer.js
```

![](https://i.imgur.com/F2Kngkk.jpg)

- 이제는 인스턴스를 종료하여도 Server가 작동하게 된다.

- 추가 명령어
```linux
# PM2 상태 확인
$ pm2 status

# PM2 전체 종료
$ pm2 kill

# PM2 특정 App 종료 - 여기서는 socketServer 가 AppName
$ pm2 stop AppName 

```


### Git Action workflows 작성

- 해당 Git Repository에 `workflows` 작성
>`.github/workflows` 디렉토리에 YAML 파일 형식으로 작성됩니다.<br>이 파일은 빌드 프로세스를 자동화하고, 특정 이벤트(예: 푸시, 풀 리퀘스트)에 반응하여 실행되는<br>일련의 작업을 정의합니다.

A) workflows 작성
- name은 파일의 이름이 됩니다 여기서는 `server-cicd.yml`

```yml
name: Server CI/CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v2

    - name: Deploy to EC2
      uses: appleboy/scp-action@master
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USER }}
        key: ${{ secrets.EC2_SSH_KEY }}
        port: 22
        source: "socketServer.js"
        target: "/home/ec2-user/rpg-socket-server"

    - name: Run PM2
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USER }}
        key: ${{ secrets.EC2_SSH_KEY }}
        port: 22
        script: |
          cd /home/ec2-user/rpg-socket-server
          pm2 start socketServer.js --name rpg-socket-server || pm2 restart rpg-socket-server
```

### Git Secret 세팅
- 외부에 Key 및 Host 등 민감한 정보에 대해서는 Git 자체에서 비밀스럽게 관리가 가능한 Secet을 사용
- Git 해당 Repository => Settings => 왼쪽의 Security => Secrets and variable => Actions

![](https://i.imgur.com/SsqKxez.jpg)

- 이곳에서 `New repository secret`를 눌러 해당 내용들을 기입하여 줍니다.

### AWS EC2 Setting
- 
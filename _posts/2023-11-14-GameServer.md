---
layout: post
title: GameServer 구축
subtitle: Amazon Linux 인스턴스에 Server 띄우기
author: Daniel
categories: Project
tags: 
 - Network
 - GameServer
 - NodeJS
 - Cloud
 - Amazon
 - Csharp
 - Server
banner:
  image:
---

Gama Server Build
--

### 1. Aws Instance Create

- 사용하는 Server App의 아키텍쳐에 맞추어 인스턴스 생성
- 무료이고 크게 속도가 필요 없으며 테스트용의 용도에 맞게 t2.micro 로 인스턴스 생성

|목록|내용|
|:--|:--|
|Provider|Amazon AWS|
|Instance<br>Machine|Amazon Linux 2023 AMI|
|Instance<br>Arcitacture|x86, arm|
|Instance<br>Category|t2.micro (free tier)|
|Instance Spec|1 vCPU, 1 GiB Memory, 8GB Storage|

![](https://i.imgur.com/jeWRVdO.jpg)

#### 1 - 1. Instace Connect Test

A) Pem Key Change to Permission 

- 최초 발급 받은 pem Key의 경우 거의 다 서버 접근을 하려고 하면 Permission Denied 가 발생하기 때문

```linux
daniel@Danielui-MacBookPro ~ % ssh -i /Users/daniel/Desktop/GameServer.pem ec2-user@your-server-public-ip

The authenticity of host 'your-server-public-ip' can't be established.

ED25519 key fingerprint is SHA256:bL+C3q646IZa4Eguia79418UHbeMbWowY4by8R9KF7U.

This key is not known by any other names.

Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added 'your-server-public-ip' (ED25519) to the list of known hosts.

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Permissions 0644 for '/Users/daniel/Desktop/GameServer.pem' are too open.

It is required that your private key files are NOT accessible by others.

This private key will be ignored.

Load key "/Users/daniel/Desktop/GameServer.pem": bad permissions

ec2-user@52.78.28.10: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

- pem key의 위치를 확인 후 해당 pem Key의 경로를 입력하거나 Key를 Drag && Drop 하면 된다

```linux
daniel@Danielui-MacBookPro Desktop % chmon 400 /Users/daniel/Desktop/GameServer.pem
```

```linux
$ cmod 400 pemKey-path
```


B) Server Connect

- Key 권한 설정이 끝났다면 해당 Pem 키를 이용해 서버에 접속하자
- AWS Server의 경우 다음과 같이 접근할 수 있다

```linux
$ ssh -i pemKey-Path ec2-user@public-IP
```

- AWS Linux의 경우 볼수 있는 접속 화면

```linux
daniel@Danielui-MacBookPro Desktop % ssh -i /Users/daniel/Desktop/GameServer.pem ec2-user@your-server-public-ip

   ,     #_

   ~\_  ####_        Amazon Linux 2023

  ~~  \_#####\

  ~~     \###|

  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023

   ~~       V~' '->

    ~~~         /

      ~~._.   _/

         _/ _/

       _/m/'
```

### 2. Node JS Engine Install

- AWS Instance의 경우 처음 인스턴스 생성시 비어있기 때문에 세팅일 필요
- 지금 필요한 Program은 다음과 같다
	- Node JS App
	- postgresql DataBase
	- 무중단 배포를 위한 PM2

#### Node 설치 
- 현재 LTS버전 보다 많이 떨어질 수 있는 패키지가 설치 될 수 있으니 호환성을 체크하기를 요망

A) node 설치 전 Instance의 설치 가능한 Package Manager를 업데이트 해준다 
- AWS Linux의 경우 `yum`을 사용한다

```linux
sudo yum update -y
```

- sudo : 관리자 권한
- yum : Package Manager
- update : 명령
- -y : 중간 중간 의사를 묻는 분기가 나오는데 이떄 모두 일관 yes 처리를 하겠다는 보조명령어 <br>(작성한하면 분기에서 멈추어 의사를 물어봅니다.)

![](https://i.imgur.com/99akyJ5.jpg)

B) 설치 가능한 패키지 보기
- 전체 설치 가능한 리스트를 보려면
- 직접 실행해보면 엄청나게 많은 리스트가 나온다

```linux
yum list available
```

![](https://i.imgur.com/BUfMthP.jpg)

- 원하는 설치 파일 목록만 보기

```linux
sudo yum available | grep 패키지이름
```

> 여기서는 버전 이슈에 대응하기 위해 별도의 NVM을 설치하기로 함

[nvm Github](https://github.com/nvm-sh/nvm#install--update-script)해당 링크에서 최신 nvm 버전을 확인하여 url을 통해 인스턴스에 직접 설치 하기로 한다

![](https://i.imgur.com/OZBSNVx.jpg)

- Linux 의 경우 두개 다 사용 가능하다 여기서는 두번째 `wget`링크를 이용한다 

```linux
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

![](https://i.imgur.com/ml68wCS.jpg)

- 설치 이후 변경사항을 적용 시켜준다

```linux
[ec2-user@ip-172-31-38-37 ~]$ source ~/.bashrc
```

- nvm 으로 최신 Node JS LTS 버전 설치하기

```linux
[ec2-user@ip-172-31-38-37 ~]$ nvm install --lts
```

- 만약 원하는 버전이 있다고 하면 

```linux
# node의 버전을 명시
nvm install 18.14.0

# LTS에 아닌 제공하는 최신버전
nvm install node
```


![](https://i.imgur.com/i3pCa3s.jpg)

- 설치된 Node 및 npm Version 확인

```linux
node -v
npm -v
```

![](https://i.imgur.com/yMjDNoB.jpg)

### Node 프로젝트 인스턴스에 옮기기

- 많은 방법이 있지만 FileZilla를 이용한 방법으로 Node JS 프로젝트를 Instance에 설치 해보겠습니다.

A) File Zilla Install
- [FileZilla - Client](https://filezilla-project.org/)에서 Client를 설치하고 실행 

![](https://i.imgur.com/J2kguds.jpg)


![](https://i.imgur.com/objzQ5L.jpg){: .left-block :}

- 좌측 상단에 `사이트`관리자를 선택하고
- 관리자 화면에서 새 사이트 생성

![](https://i.imgur.com/29MVq2b.jpg)

- 우측 세팅 
- 프로토콜 : SFTP - SSH File Transfer Protocol
- 호스트 : 인스턴스 접근 Public IP (포트 공란)
- 로그온 유형 : 키 파일
- 사용자 : ec2-user
- 키 파일 => 찾아보기 => pem Key File Path

![](https://i.imgur.com/aOh5cha.jpg)

- 연결 상태 

![](https://i.imgur.com/sQAavXO.jpg)

- 이곳에서 Local 위치에서 제작한 프로젝트를 해당 Ec2 Instance에 Drag && Drop 방식으로 파일을 옮길 수 있습니다. ❗️ 경로는 특별한 이유가 없다면 ec2-user Path 에서 하시기를 권장 드립니다.
- 파일 전송이 끝났다면 인스턴스에 정상적으로 들어왔는지 확인 합니다.

```linux
# 해당 인스턴스 Path에 있는 목록 출력
ls
```

![](https://i.imgur.com/6LOmxkC.jpg)

### Server Test

- 여기 까지 모두 완료 하였다면 해당 프로젝트를 실행 하고 web에서 접속 시도를 해봅니다.
 
![](https://i.imgur.com/xpY9d2r.jpg)

- 웹에서도 접속 시도를 해봅니다

![](https://i.imgur.com/K6MpAGp.jpg)

- 네 접속이 되지 않습니다.
- AWS의 보안 탭을 확인해 봅니다.

![](https://i.imgur.com/0u9rMBu.jpg)

- 인바운드 규칙 중에 지금 Node 프로젝트의 Port인 3000 을 설정 하지 않았습니다. 
- 인바운드 규칙 편집으로 설정해 줍니다.

![](https://i.imgur.com/rwfTtHj.jpg)

- 다시 접속을 시도 합니다.

![](https://i.imgur.com/xEcyalp.jpg)

- 정상적으로 접속 되는 것을 확인 합니다.

### Client And Server Test

- 서버가 확인 되었다면 Client와 연결해 봅니다
- 먼저 Client의 접속 TCP 를 수정합니다


![](https://i.imgur.com/c5sADmL.jpg)

![](https://i.imgur.com/drMMuvd.jpg)

![](https://i.imgur.com/2E2FpyP.gif)

- 제대로 접속 되는 것 까지 확인이 완료 되었습니다.
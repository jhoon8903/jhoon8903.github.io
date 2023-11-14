---
layout: post
title: EC2 Database 구축 
subtitle: 케릭터 정보, 아이템 정보 DB 구축하기
author: Daniel
categories: Project
tags: 
 - Network
 - Database
 - Project
 - Server
banner:
  image:
---

Game DB Build
--

- 게임의 케릭터 및 아이템 정보에 관한 DB를 구축하여 클라이언트에서 정보 분리하기
- 클라이언트에서는 Game 만 진행을 하며 필요한 케릭터 정보 및 아이템 정보등은 Server에서 전달 받아 사용

### Server Database Build
- 서버에서는 SQL 형식의 데이터를 사용하며
- Json 데이터를 가져오며 모든 데이터의 수정은 DB에서 이루어짐

## Server

```javascript

```
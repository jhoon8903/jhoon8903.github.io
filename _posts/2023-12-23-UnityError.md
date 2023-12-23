---
layout: post
title: Allocation Warning
subtitle: 알수 없는 에러
author: Daniel
categories: Unity
tags: 
 - Unity
banner:
  image: https://i.imgur.com/XiFfGnN.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

새 프로젝트 생성시 발생한 문제
--

![](https://i.imgur.com/XiFfGnN.jpg)

- 프로젝트 생성 시 발생한 문제로 해당 로그들은 다음과 같습니다.

#### Layout File Loading Issue

- "The layout ... could not be fully loaded"라는 오류는 Unity가 사용자 정의 레이아웃 파일을 로딩하는데 문제가 있음을 나타냅니다. 
- 이 문제는 레이아웃 파일이 현재 Unity 프로젝트에서 사용할 수 없거나 존재하지 않는 에디터 윈도우나 다른 컴포넌트를 참조할 때 발생할 수 있습니다.

#### Memory Allocation Warnings

- Allocation of 37 bytes..."와 같은 메모리 할당 메시지와 해제되지 않은 할당에 대한 경고는 메모리 누수 또는 부적절한 메모리 관리를 나타냅니다.

#### TLS and Stack Allocator Warnings

- 메모리 할당 경고와 유사하게, 이러한 경고는 임시 메모리 할당이 제대로 해제되지 않았음을 나타냅니다.

### 솔루션

- 프로젝트 초기화 문제시 가장 먼저 할 방법은 Reimport All 을 통해 패키지 및 플러그인을 초기화 해보는 것이 가장 빠릅니다.

![](https://i.imgur.com/dCWCpGj.jpg)

---
layout: post
title: 개인과제 프로젝트 2 Title, Login 
subtitle: Dungeon RPG 타이틀 및 로그인 기능
author: Daniel
categories: Project
tags: 
 - Csharp
 - Programming
 - Game
banner:
  image:
---

### Title
- 시작 메인화면 출력
- 로그인, 계정생성, 나가기 구성
- ASCII Art Title 구성

![](https://i.imgur.com/bWi2iHm.jpg)

> 각 입력값의 처리는 `ConsoleKeyInfo keyinfo = ReadKey` 로 작동 구성
>```csharp
> 	ConsoleKeyInfo keyInfo = ReadKey();  
> 	switch (keyInfo.Key)  
>		{  
>		case ConsoleKey.UpArrow:  
>		{  
>			selectedMenuItem--;  
>			if (selectedMenuItem < 0)  
>			{  
>				selectedMenuItem = menuItems.Length - 1;  
>			}  
>			break;  
>		}  
>		case ConsoleKey.DownArrow:  
>		{  
>			selectedMenuItem++;  
>			if (selectedMenuItem >= menuItems.Length)  
>			{  
>				selectedMenuItem = 0;  
>			}  
>			break;  
>		}  
>		case ConsoleKey.Enter:  
>		switch (selectedMenuItem)  
>		{  
>			case 0:  
>				Account.LogIn();  
>				break;  
>			case 1:  
>				Account.CreateAccount();  
>				break;  
>			case 2:  
>				Environment.Exit(0);  
>				break;  
>		}  
>		break;  
>	}


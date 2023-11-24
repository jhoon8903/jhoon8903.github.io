---
layout: post
title: C# Mac && Window 공동 작업을 위한 ConsoleSize Fixer
subtitle: Console Size Fixer 코드 공유
author: Daniel
categories: Project
tags: 
 - Programming
 - Project
 - Csharp
 - Utility
banner:
  image: https://i.imgur.com/CjIdRvd.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Console Size Fixer 제작
--

Window와 작업하면서, Mac에서는 Console 크기에 대한 설정이 불가능하여     
협업을 하면서 문제가 발생했습니다.

서로간에 사이즈를 맞추지 않고 작업을 하게되면 결과물의 인터페이스가 서로 따로 놀게되어     
결과물의 퀄리티가 떨어질 수 있기 때문에 `최소한의 크기`의 가이드가 필요했습니다.

조건은 다음과 같습니다.

### Utility의 조건

1. 최소한의 사이즈는 유지한체 더이사 벗어나지 않도록 랜더링 할 수 있는 가이드라인
2. Window Mac 모두 호환되는 Cross Platform 적용이 가능한 코드일 것
3. 팀원 모두가 쉽게 사용할 수 있어야 하는 코드

### Code Source

```csharp
using System.Runtime.InteropServices; 
namespace HexaCoreVillage.Utility 
{ 
	public static class ConsoleSizeUtility 
	{ 
		// 'libc' 라이브러리의 'system' 함수를 사용하기 위한 DLL 임포트 선언 
		[DllImport("libc")] private static extern int system(string exec); 
		// 고정된 콘솔 창의 행과 열의 크기를 상수로 선언 
		private const int FixedRows = 30; 
		private const int FixedColumns = 180; 
		
		public static void RedrawBorder() 
		{ 
			// 콘솔의 크기를 설정하는 ANSI 이스케이프 시퀀스 실행 
			system($@"printf '\e[8;{FixedRows};{FixedColumns}t'"); 
			// 테두리 그리기 위한 콘솔 전경색을 녹색으로 설정 
			ForegroundColor = ConsoleColor.Green; 
			// 콘솔의 상단 테두리를 그립니다. '=' 문자를 사용하여 상단 테두리를 그림 
			SetCursorPosition(0, 0); 
			Write(new string('=', FixedColumns)); 
			// 콘솔의 양쪽 테두리를 그립니다. '|' 문자를 사용하여 측면 테두리를 그림 
			for (int i = 1; i < FixedRows - 1; i++) 
			{ 
				SetCursorPosition(0, i); Write('|'); 
				SetCursorPosition(FixedColumns - 1, i); 
				Write('|'); 
			} 
			// 콘솔의 하단 테두리를 그립니다. '=' 문자를 사용하여 하단 테두리를 그림 
			SetCursorPosition(0, FixedRows - 1); 
			Write(new string('=', FixedColumns)); 
			// 콘솔 전경색을 초기화 
			ResetColor(); 
		} 
	} 
}
```

- 해당 코드는 고정된 횡과 열을 입력
- CursorSize를 이용하여 0번 부터 정해진 길이까지 라인을 랜더링
- 고정된 사이즈의 박스를 Loop의 마지막에 넣으면 반복해서 랜더링 하게 됨
- 콘솔의 최소 가이드라인을 생성하여 이후 UI 작업에 용이해짐

![](https://i.imgur.com/CjIdRvd.gif)

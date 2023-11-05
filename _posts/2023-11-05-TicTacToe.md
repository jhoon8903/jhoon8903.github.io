---
title: C# TicTacToe Console Game
author: "이정훈"
category: C#
tags: [Programming, csharp, Game]
img : https://i.imgur.com/lhEEBGQ.gif
comments_disable: true
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

# Console TicTacToe 만들기

## Game Logic

```csharp
public static class TicTacToe()
{
	private static char[,]? _board;
	private const int Size = 3;
	private static readonly Random Random = new Random();

	// TicTacToe 게임 실행
	public static void TicTacToeGame()
	{
		// 보드를 생성할 size의 보드 할당
		_board = new char[Size,Size];
		// 보드 초기화
		InitBoard();
		// Console에 Board를 표시하기
		DisplayBoard();
		// 플레이 하기
		Play();
	}
}
```

## InitBoard()

- 2차원 배열을 순회하여 보드 사이 사이에 빈 공백을 넣어 게임 보드를 초기화 
- 총 9칸의 공백이 생성 됨

|1|2|3|
|:--:|:--:|:--:|
|4|5|6|
|7|8|9|

```csharp
private static void InitBoard()
{
	for(int i = 0; i < Size; i++)
	{
		for(int j = 0; j < Size; j++)
		{
			if (_board != null) 
			{
				_board[i,j] = ' ';
			}
		}
	}
}
```

## DisplayBoard()

- 플레이어가 보드를 볼 수 있도록 시각화하여 구분

```csharp
private static void DisplayBoard()
{
	for (int i = 0; i < Size; i++)  
	{  
		for (int j = 0; j < Size; j++)  
		{  
			if (_board != null)  
			{  
				Console.Write(_board[i, j]);  
			}  
			if (j < Size - 1)  
			{  
				Console.Write("|");  
			}  
		}  
		Console.WriteLine();  
		if (i < Size - 1)  
		{  
			Console.WriteLine("-----");  
		}  
	}
}
```

## Play()

- 게임 플레이 로직 
- 유저턴과 컴퓨터 턴을 구분하여 번갈아 가면서 실행
- Console.ReadLine 입력을 받아 디스플레이에 표시

```csharp
private static void Play()  
{  
	bool isPlayerTurn = true; // 플레이어부터 시작  
	char winner = ' ';  
	while (true) // 또는 승리 조건이 충족될 때까지 루프  
	{  
		if (isPlayerTurn)  
		{  
			Console.Write("공격할 좌표 (예: 0,1) 
							\n[0~2]까지 입력 가능 Player는 'X' 입니다.: ");  
			string input = Console.ReadLine();  
			string[] separate = input.Split(',');  
		  
			if (separate.Length != 2 || !int.TryParse(separate[0], out int row)
			|| !int.TryParse(separate[1], out int col)||row < 0||row >= Size
			||col < 0 || col >= Size ||_board[row, col] != ' ')  
			{  
				Console.WriteLine("잘못된 입력입니다. 다시 시도하세요.");  
				continue;  
			}  
		  
			_board[row, col] = 'X'; // 플레이어의 심볼을 'X'로 가정  
			DisplayBoard();  
			winner = CheckForWinner();  
			if (winner != ' ')  
			{  
				Console.WriteLine(winner + " 플레이어가 승리했습니다!");  
				break;  
			}  
			else if (IsBoardFull())  
			{  
				Console.WriteLine("무승부입니다!");  
				break;  
			}  
		}  
		else // 컴퓨터의 턴  
		{  
			TakeComputerTurn();  
			DisplayBoard();  
			winner = CheckForWinner();  
			if (winner != ' ')  
			{  
				Console.WriteLine(winner + " 컴퓨터가 승리했습니다!");  
				break;  
			}  
			else if (IsBoardFull())  
			{  
				Console.WriteLine("무승부입니다!");  
				break;  
			}  
		}  
	  
	// 턴 교대  
	isPlayerTurn = !isPlayerTurn;  
	}  
}
```

```csharp
private static void TakeComputerTurn()  
{  
	while (true)  
	{  
		// Random한 ' ' 위치에 'O'표기
		int row = Random.Next(Size);  
		int col = Random.Next(Size);  
		  
		if (_board[row, col] != ' ') continue;  
		_board[row, col] = 'O';  
		break;  
	}  
}  
  
private static char CheckForWinner()  
{  
	// 행 검사  
	for (int row = 0; row < Size; row++)  
	{  
		if (_board[row, 0] != ' ' && _board[row, 0] == _board[row, 1] 
			&&  _board[row, 1] == _board[row, 2])  
		{  
			return _board[row, 0];  
		}  
	}  
	  
	// 열 검사  
	for (int col = 0; col < Size; col++)  
	{  
		if (_board[0, col] != ' ' && _board[0, col] == _board[1, col] 
			&&  _board[1, col] == _board[2, col])  
		{  
			return _board[0, col];  
		}  
	}  
	  
	// 대각선 검사  
	if (_board[0, 0] != ' ' && _board[0, 0] == _board[1, 1] && 
		_board[1, 1] == _board[2, 2])  
	{  
		return _board[0, 0];  
	}  
	  
	if (_board[0, 2] != ' ' &&  _board[0, 2] == _board[1, 1] &&  
		_board[1, 1] == _board[2, 0])  
	{  
		return _board[0, 2];  
	}  
	  
	// 승자가 없는 경우  
	return ' ';  
}  
  
private static bool IsBoardFull()  
{  
	for (int row = 0; row < Size; row++)  
	{  
		for (int col = 0; col < Size; col++)  
		{  
			if (_board[row, col] == ' ')  
			{  
				return false;  
			}  
		}  
	}  
	return true;  
}
```


![](https://i.imgur.com/lhEEBGQ.gif)

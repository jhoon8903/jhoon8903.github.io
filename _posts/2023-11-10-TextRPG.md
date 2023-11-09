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
 - Script
banner:
  image: https://i.imgur.com/Rhhu40U.gif
---

![](https://i.imgur.com/yA3gBz9.png)


Title 구성
--
## Title
- 시작 메인화면 출력
- 로그인, 계정생성, 나가기 구성
- ASCII Art Title 구성
	- ASCII 제작은 [Text to ASCII Generator](patorjk.com) 이용

![](https://i.imgur.com/bWi2iHm.jpg)

> 각 입력값의 처리는 `ConsoleKeyInfo keyinfo = ReadKey` 로 작동 구성
> ConsoleKeyInfo : 콘솔 키로 표현된 문자, Shift, Alt, Ctrl 보조키의 상태를 포함하여 누른 콘솔 키를 반환 

```csharp
> public readonly struct ConsoleKeyInfo : IEquatable<ConsoleKeyInfo>
```

> ConsoleKey의 경우 열거형 (Enum) 타입으로 콘솔의 `표준키'를 지정함
> 해당 필드는 [MS .NET7](https://learn.microsoft.com/ko-kr/dotnet/api/system.consolekey?view=net-7.0)를 참고.

```csharp
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
```


![](https://i.imgur.com/ImpHpWe.gif)

```csharp
for (int i = 0; i < menuItems.Length; i++)  
{  
PrintLine($"\t\t\t██████ {menuItems[i]} ██████", i == selectedMenuItem ? ConsoleColor.Yellow : ConsoleColor.Gray);  
}
```
- 해당 Loop를 순회하면서 선택된 menuItem의 색상을 변경한다.



---


로그인 및 계정생성 기능
--
## Create Account

1. 데이터 파싱
	*  최초 User.Json 파일을 로드하여 `역직렬화(Deserialize)`하여 Json Data를 Uses List에 저장   

	```csharp
	public static List<Character>? Users = new ();  
  
	public static void LoadAccountData()  
	{  
		string jsonString = File.ReadAllText(GameManager.CharacterFilePath);  
		if (jsonString != null)  
		{  
			Users = JsonSerializer.Deserialize<List<Character>>(jsonString);  
		}  
	}
	```

### Json 직렬화 역직렬화

#### 직렬화
- 데이터의 구조나 객체 상태를 문자열로 변환
- 변환된 문자열은 파일에저장 또는 네트워크를 통해 전송 가능

#### 역직렬화
- JSON 문자열을 데이터 구조나 객체로 변환

```json
{  
	"id": "Master",  
	"pw": "uXSnqOiXKs.........asdqwererv4lp-[-0ijjf,  
	"level": 1,  
	"exp": 0,  
	"job": 5,  
	"damage": 10,  
	"itemDamage": 0,  
	"defence": 5,  
	"itemDefence": 0,  
	"hp": 100,  
	"gold": 1527,  
	"inventory": 
	{  
		"무쇠갑옷": 
		{  
			"toOwn": true,  
			"equipment": false  
		},  
		"낡은 검": 
		{  
			"toOwn": true,  
			"equipment": false  
		}  
	}  
},
```

- 역직렬화 후 데이터를 조회하면 객체의 Data를 출력 할 수 있다.   

```csharp
Console.WriteLine(Users[0].Id); // "Master" 
Console.WriteLine(Users[0].Gold); // 1527
```

### ID 중복 확인    

```csharp
public static void CreateAccount()  
{  
	while (true)  
	{  
		ForegroundColor = ConsoleColor.Cyan;  
		WriteLine("Create Account");  
		WriteLine("0. 나가기");  
		WriteLine("\n");  
		ResetColor();  
		Write("생성할 ID를 입력하세여 (영) >> ");  
		string id = ReadLine();  
		Character? user = Users.FirstOrDefault(u => u.Id == id);  
		if (id == "0")  
		{  
			Title.MainTitle();  
		}  
		if (user!= null && id != "0")  
		{  
			ForegroundColor = ConsoleColor.Red;  
			WriteLine("이미 존재하는 계정입니다.");  
			ResetColor();  
		}  
		else  
		{  
			var pw = InputPassword();  
			SaveAccount(id, pw);  
		}  
	}  
}
```

- 역 직렬화된 데이터를 `FirstOrDefault`를 이용하여 입력된 Id와 Json Data의  Id를 비교하여 검증
- 중복되는 ID가 없다면 PW를 입력 받아 데이터를 저장   

### PW 입력   
   
```csharp
private static string InputPassword()  
{  
	string finalPassword = "";  
	Write("비밀번호를 입력하세요 (영/숫자) >> ");  
	string pw = ReadLine();  
	  
	Write("(확인)동일한 비밀번호를 다시 입력해주세요 >> ");  
	string checkPw = ReadLine();  
	  
	if (pw != checkPw)  
	{  
		ForegroundColor = ConsoleColor.Yellow;  
		WriteLine("비밀번호가 일치하지 않습니다. 다시 확인해주세요.");  
		ResetColor();  
		InputPassword();  
	}  
	else  
	{  
		finalPassword = pw;  
	}  
	return finalPassword;  
}
```


### Data 저장   

```csharp
private static void SaveAccount(string id, string pw)  
{  
	string hashedPassword = HashPassword(pw);  
	const string accountFilePath = GameManager.CharacterFilePath;
	string jsonString = File.ReadAllText(accountFilePath, Encoding.UTF8);  
	List<Character> accounts 
		= JsonSerializer.Deserialize<List<Character>>(jsonString) 
		?? new List<Character>();  
		
	Character newAccount = new Character  
	{  
		Id = id,  
		Pw = hashedPassword,  
		Level = 1,  
		Exp = 0.0f,  
		Job = Character.Jobs.무직,  
		Damage = 10,  
		ItemDamage = 0,  
		Defence = 5,  
		ItemDefence = 0,  
		HealthPoint = 100.0f,  
		Gold = 1500,  
		Inventory = new Dictionary<string, InventoryItem>  
		{  
			{ "무쇠갑옷", new InventoryItem { ToOwn = true, Equipment = false } }, 
			{ "낡은 검", new InventoryItem { ToOwn = true, Equipment = false } }  
		}  
	};  
	accounts.Add(newAccount);  
	
	var options = new JsonSerializerOptions  
	{  
		WriteIndented = true,  
		Encoder = JavaScriptEncoder.UnsafeRelaxedJsonEscaping  
	};  
	jsonString = JsonSerializer.Serialize(accounts, options);  
	File.WriteAllText(accountFilePath, jsonString, Encoding.UTF8);  
	WriteLine("계정이 생성 되었습니다. 로그인 해 주세요");  
	while (true)  
	{  
		WriteLine("Enter를 입력하면 타이틀 화면으로 돌아갑니다.");  
		ConsoleKeyInfo inputKey = ReadKey();  
		if (inputKey.Key == ConsoleKey.Enter)  
		{  
			Program.Main();  
			break;  
		}  
		WriteLine("입력값이 올바르지 않습니다.");  
	}  
}
```

- id와 pw를 매개변수로 전달
- 기본 Json 세팅값을 생성
- 직렬화를 하여 Json 객체 형태로 만들어 Json 파일에 저장   

```csharp
jsonString = JsonSerializer.Serialize(accounts, options);  
File.WriteAllText(accountFilePath, jsonString, Encoding.UTF8); 
```

🖍️ Encoding을 UTF8로 별도의 옵션을 사용하지 않으면, 한글의 경우 JSON내부에서 꺠지는 현상이 발생
역직렬화 할때는 정상적으로 변환 되지만, 데이터 관리를 위해 인코딩을 하는 것을 권장

### PW 암호화
[Argon2를 이용한 단방향 암호화 참조](https://jhoon8903.github.io/c%23/2023/11/07/2-%EC%95%94%ED%98%B8%ED%99%94.html)


![](https://i.imgur.com/Rhhu40U.gif)

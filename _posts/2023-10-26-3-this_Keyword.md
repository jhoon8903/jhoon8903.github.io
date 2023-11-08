---
title: C# Class this Keyword
category: C#
author: 이정훈
tags: [Language, Programming]
img: https://i.imgur.com/m8yClWp.png
comments_disable: true
meta_description: C# Class this Keyword
---
# This
- 객체가 자신을 지칭할 때 사용하는 키워드
- 객체 외부에서는 개체의 필드나 메소드에 접근할 때 개개체의 이름을 사용(변수, 식별자 등)
- 객체 내부에서는 자신의 필드나 메소드에 접근할 때 `this` 키워드 사용
```csharp
class Employee
{
	private string Name;

	public void SetName(string Name)
	{
		this.Name = Name;
	}
}
```
- this를 사용시 `this.Name`은 Employee 자신의 필드, `Name`은 SetName() 메소드의 매개변수 가 됨

```csharp
public class ThisKeyword  
{  
	private string Name;  
	private string Position;  
  
	public void SetName(string Name)  
	{  
		this.Name = Name;  
	}  
	  
	public string GetName()  
	{  
		return Name;  
	}  
	  
	public void SetPosition(string Position)  
	{  
		this.Position = Position;  
	}  
	  
	public string GetPosition()  
	{  
		return this.Position;  
	}  
	  
	public static void Main()  
	{  
		ThisKeyword pooh = new ThisKeyword();  
		pooh.SetName("Pooh");  
		pooh.SetPosition("Waiter");  
		Console.WriteLine($"{pooh.GetName()} {pooh.GetPosition()}");  
		  
		ThisKeyword tigger = new ThisKeyword();  
		tigger.SetName("Tigger");  
		tigger.SetPosition("Cleaner");  
		Console.WriteLine($"{tigger.GetName()} {tigger.GetPosition()}");  
	}  
}
```
![](https://i.imgur.com/XeO38QV.jpg)


--- 


# This ()
- 자기 자신의 생성자를 가리킴
- 생성자에서만 사용할 수 있음, 코드 블록 내부가 아닌 앞쪽에서만
```csharp
public class ThisConstructor  
{  
	private int a, b, c;  
	  
	public ThisConstructor()  
	{  
		this.a = 5425;  
	}  
	  
	public ThisConstructor(int b)  
	{  
		this.a = 5435;  
		this.b = b;  
	}  
	  
	public ThisConstructor(int b, int c)  
	{  
		this.a = 5425;  
		this.b = b;  
		this.c = c;  
	}  
}

/ This() 생성자를 이용한 코드 최적화
public class ThisConstructor  
{  
	private int a, b, c;  
  
	public ThisConstructor()  
	{  
		this.a = 5435;  
	}  

	/ ThisConstructor()를 호출
	public ThisConstructor(int b) : this()  
	{  
		this.b = b;  
	}  

	/ ThisConstructor(int b)를 호출
	public ThisConstructor(int b, int c) : this(b)
	{  
		this.c = c;  
	}  
}
```

```csharp
public class ThisConstructor  
{  
	private int a, b, c;  
	public ThisConstructor()  
	{  
		this.a = 5435;  
		Console.WriteLine("MyClass()");  
	}  
	  
	public ThisConstructor(int b) : this()  
	{  
		this.b = b;  
		Console.WriteLine($"MyClass({b})");  
	}  
	  
	public ThisConstructor(int b, int c) : this(b)  
	{  
		this.c = c;  
		Console.WriteLine($"MyClass({b}, {c})");  
	}  
	  
	public void PrintFields()  
	{  
		Console.WriteLine($"a : {a}, b : {b}, c : {c}");  
	}  
	  
	public static void App()  
	{  
		ThisConstructor a = new ThisConstructor();  
		a.PrintFields();  
		Console.WriteLine();  
		  
		ThisConstructor b = new ThisConstructor(1);  
		b.PrintFields();  
		Console.WriteLine();  
		  
		ThisConstructor c = new ThisConstructor(10,20);  
		c.PrintFields();  
	}  
	  
	public static void Main()  
	{  
		App();  
	}  
}
```
![](https://i.imgur.com/YKF119B.jpg)
---
title: C# 객체 복사
category: C#
author: 이정훈
tags:
  - csharp
  - cs
img: https://i.imgur.com/m8yClWp.png
comments_disable: true
meta_description: "C# 객체 복사 : 얕은 복사와 깊은 복사"
---
# 객체 복사하기 : 얕은 복사와 깊은 복사
## 얕은 복사 (Shallow Copy)
```csharp
internal class MyClass  
{  
	public int MyField1;  
	public int MyField2;  

	public static void ShallowCopy()  
	{  
		MyClass source = new MyClass();  
		source.MyField1 = 10;  
		source.MyField2 = 20;  
		  
		MyClass target = source;  
		target.MyField2 = 30;  
		  
		Console.WriteLine("Shallow Copy");  
		Console.WriteLine($"Source1 : {source.MyField1}, 
							Source2 : {source.MyField2}");  
		Console.WriteLine($"target1 : {target.MyField1}, 
							target2 : {target.MyField2}");  
		Console.WriteLine();  
	}
}  
```
![](https://i.imgur.com/X64QvCB.jpg)

1. 변경하지 않은 `Source2`가 변한 이유는 `Class` 는 `참조` 형식 이기 때문
	힙 영역에 객체를 할당하고, 스택에 있는 참조가 힙 영역에 할당된 메모리를 가리킴
![](https://i.imgur.com/ZBz6ySs.jpg)

2. source2를 복사하여 받은 target2는 `힙에 있는 실제 객체가 아닌 스택에 있는 참조를 복사`하여 받음
![](https://i.imgur.com/9mVUlcq.jpg)

3. 이 때문에 target.MyField2를 변경하였을때 source.MyField2도 같이 바뀌는 문제 발생
4. 객체를 복사할때 참조만 복사하는 것을 `얕은 복사 (Shallow Copy)라고 함`


## 깊은 복사 (Deep Copy)
```csharp
public MyClass DeepCopyObject()  
{  
	// source로 부터 별도의 힙 공간에 새로운 객체저장
	MyClass newCopy = new MyClass();  
	newCopy.MyField1 = this.MyField1;  
	newCopy.MyField2 = this.MyField2;  
	return newCopy;  
}

public static MyClass DeepCopy()  
{  
	MyClass source = new MyClass();  
	source.MyField1 = 10;  
	source.MyField2 = 20;  
	  
	MyClass target = source.DeepCopyObject();  
	target.MyField2 = 30;  
	  
	Console.WriteLine("Deep Copy");  
	Console.WriteLine($"Source1 : {source.MyField1}, 
						Source2 : {source.MyField2}");  
	Console.WriteLine($"target1 : {target.MyField1}, 
						target2 : {target.MyField2}");  
	return target;  
}
```
![](https://i.imgur.com/G6CJQzH.jpg)

1. target.MyField2이 힙에 보관되어 있는 내용을 source로 부터 복사하여 별도의 힙 공간에 객체를 보관
![](https://i.imgur.com/ShGFoNk.jpg)

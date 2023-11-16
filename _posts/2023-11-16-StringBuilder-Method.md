---
layout: post
title: String Builder 메서드
subtitle: String Builder의 주요 메서드 설명
author: Daniel
categories: Csharp
tags: 
 - Programming
 - Csharp
 - Language
banner:
  image:
---

String Builder Method
--

### Append

- 현재 `StringBuilder`의 `끝`에 문자열을 추가

```csharp
StringBuilder sb = new StringBuilder("Hello");
ab.Append(" World")
Console.WriteLine(sb);

결괏 값 => "Hello World"
```

### AppendLine

- 지정된 문자열과 기본 줄 종결자를 현재 `StringBuilder` 객체의 끝에 추가 합니다.

```csharp
StringBuilder sb = new StringBuilder("Hello");
sb.AppendLine(" World");
Console.WriteLine(sb);

결괏 값 => "Hello World\n"
```

### Insert

- 문자열이나 객체를 현재 `StringBuilder`객체의 `지정된 인덱스에 삽입`

```csharp
String builder sb = new StringBuilder("Hello World");
sb.Insert(6, "Beautiful ");
Console.WriteLine(sb);

결괏 값 => "Hello Beautiful World"
```

### Remove

- 현재 `StringBuilder` 객체에서 지정된 범위의 문자를 제거 합니다.

```csharp
StringBuilder sb = new StringBuilder("Hello Beautiful World");
sb.Remove(6, 10);
Console.WriteLine(sb);

결괏 값 => "Hello World"
```

### Replace

- 현재 `StringBuilder` 객체에서 지정된 문자나 문자열을 다른 지정된 문자나 문자열로 모두 교체

```csharp
StinrgBuilder sb = new StringBuilder("Hello World");
sb.Replace("World", "Universe");
Console.WriteLine(sb);

결괏 값 => "Hello Universe"
```

### Clear

- 현재 `StringBuilder` 객체에서 문자를 제거합니다.

```csharp
StringBuilder sb = new StringBuilder("Hello Wolrd");
sb.Clear();
Console.WriteLine(sb);

결괏 값 => ""
```

### ToString

- `String Builder` 객체의 값을 `String` 객체로 변환

```csharp
StringBuilder sb = new StringBuidler("Hello World");
string str = sb.ToString();
Console.WriteLine(str);

결괏 값 => "Hello World"
```
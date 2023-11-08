---
title: ASCII Char 문자 다루기
author: "이정훈"
category: Algorithm
tags: [csharp, Algorithm, Programming]
img :
comments_disable: true
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

# 프로그래머스 - 자연수 뒤집어 배열로 만들기

> 문제 
> 
> 자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 
> 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.
> 
> 제한 조건
> - n은 10,000,000,000이하인 자연수입니다.

입출력 예

|n|return|
|---|---|
|12345|[5,4,3,2,1]|

```csharp
public class Solution 
{
	public int[] solution(long n) 
	{
		string digits = n.ToString();
		
		int[] reversedNumbers = new int[digits.Length];
		
		for (int i = 0; i < digits.Length; i++)
		{
			reversedNumbers[i] = digits[digits.Length -1 - i] -'0';
		}
		
		return reversedNumbers;
	}
}
```


## 왜 '0'을 빼주는가?

컴퓨터는 내부적으로 모든 것을 숫자로 처리합니다. 
이는 문자도 예외가 아니며, 각 문자는 숫자로 된 코드에 대응됩니다. 
이 코드들은 ASCII(미국 정보 교환 표준 코드)나 UTF-8과 같은 문자 인코딩 표준을 통해 관리됩니다.

ASCII 표준에 따르면, 숫자 0부터 9까지의 문자는 연속적인 코드 값을 가지며, '0'은 48, '1'은 49, 이런 식으로 '9'는 57의 코드 값을 가집니다. 

따라서 문자 '0'에서 '0'을 빼면 0이 되고, '1'에서 '0'을 빼면 1이 되는 식으로 문자를 해당하는 정수 값으로 변환할 수 있습니다.

|ASCII|숫자|
|:--:|:--:|
|'9'|57|
|'8'|56|
|'7'|55|
|'6'|54|
|'5'|53|
|'4'|52|
|'3'|51|
|'2'|50|
|'1'|49|
|'0'|48|

```csharp
'0' - '0' = 48 - 48 = 0
'1' - '0' = 49 - 48 = 1
'2' - '0' = 50 - 48 = 2
...
'9' - '0' = 57 - 48 = 9
```


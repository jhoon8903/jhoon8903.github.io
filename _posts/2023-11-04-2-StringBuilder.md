---
layout: post
title: C# StringBuilder
subtitle: String Bulider로 알고리즘 해결하기
author: Daniel
categories: Algorithm
tags: 
 - Programming
 - Csharp
 - Algorithm
banner: 
 image : https://i.imgur.com/s1P2rRc.jpg
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

StringBuilder Class
--

## Efficiency (효율성)

>C# 에서 문자열은 불변
즉, 문자열을 생성한 후에는 변경할 수 없음
문자열을 수정하는 것처럼 보이는 작업은 실제로 새 문자열을 생성하는 것
이것의 문제는 문자열을 연결하는 등 복잡한 문자열 작업을 할 때 상당한 성능 오버헤드를 초래할 수 있음
`StringBuidler`는 가변적으로 인스턴스를 생성하지 않고도 변경할  수 있음
자주 수정해야하는 작업에 대해 효율적인 작업 방법 

## Capacity (용량)

> Capacity 속성이 있고, StringBuilder가 포함 할 수 있는 문자의 수를 나타냄
> 내용이 현재 용량을 초과하면, 보통 메모리 할당량을 두 배로 늘려 자동으로 증가
> 불변 문자열을 반복적으로 연결하는 것보다 더 효울적인 메모리 사용패턴을 제공

## Method (메소드)

```csharp
`Append(string value)`: 지정된 문자열을 `StringBuilder`의 끝에 추가합니다.

`AppendLine(string value)`: 지정된 문자열을 끝에 추가하고 줄 바꿈을 추가합니다.

`AppendFormat(string format, params object[] args)`: 형식화된 텍스트를 추가합니다.

`Insert(int index, string value)`: 지정된 인덱스 위치에 문자열을 삽입합니다.

`Remove(int startIndex, int length)`: 특정 인덱스에서 시작하여 지정된 수의 문자를 제거합니다.

`Replace(char oldChar, char newChar)`: 지정된 문자를 다른 문자로 모두 대체합니다.

`Clear()`: `StringBuilder`에서 모든 문자를 제거합니다.
```

## Performance Considerations (성능 고려사항)

> 문자열에 대한 많은 수정을 예상할 때, 특히 `Loop` 사용시 사용
> 필요한 크기를 추정할 수 있다면 미리설정하여 내부 버퍼의 크기가 조정되는 횟수 최소화
> `ToString() 호출 최소화` 호출 마다 새로운 인스턴스를 생성함

```csharp
using System;
using System.Text;

class Program
{
    static void Main()
    {
        StringBuilder sb = new StringBuilder("초기 문자열. ");

        // 문자열 추가
        sb.Append("더 많은 텍스트를 추가. ");

        // 문자 대체
        sb.Replace('.', '!');

        // 문자 제거
        sb.Remove(10, 3);

        // 특정 인덱스에 문자열 삽입
        sb.Insert(0, "시작: ");

        Console.WriteLine(sb.ToString());
    }
}
```


## `StringBuilder` vs `String`

#### When Use To `StringBuilder`
> 큰 문자열을 다루거나 빈번한 조작이 필요할 때
> 반복문 내에서 문자열을 연결할 때

#### When Use To `String`
> 자주 변경되지 않는 소규모에서 중규모의 문자열을 다룰 때
> 한 번이나 몇 번의 연결을 수행할 때


--- 

# 이상한 문자 만들기 
- 프로그래머스

## 문제 설명

문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

### 제한 사항

- 문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.
- 첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.

### 입출력 예

|s|return|
|---|---|
|"try hello world"|"TrY HeLlO WoRlD"|

### 입출력 예 설명

"try hello world"는 세 단어 "try", "hello", "world"로 구성되어 있습니다. 각 단어의 짝수번째 문자를 대문자로, 홀수번째 문자를 소문자로 바꾸면 "TrY", "HeLlO", "WoRlD"입니다. 따라서 "TrY HeLlO WoRlD" 를 리턴합니다.


```csharp
using System.Text;
public class Solution 
{
    public string solution(string s)
    {
        string[] strangeWords = s.Split(" ");
        
        StringBuilder result = new StringBuilder();

        foreach (var word in strangeWords)
        {
            char[] chars = word.ToCharArray();
           
			for (int i = 0; i < chars.Length; i++)
            {
                chars[i] = i % 2 == 0 
			                ? char.ToUpper(chars[i]) 
			                : char.ToLower(chars[i]); 
            }

            result.Append(new string(chars));
            result.Append(' ');
        }
        result.Length -= 1;
        return result.ToString();
    }
}
```


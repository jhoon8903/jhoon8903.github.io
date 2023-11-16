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
  image: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR79A7QiHWlECgG0Wm6IezYs_CjWxu68lgBaw&usqp=CAU
---
String Builder Method
--
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fddd4d8dc-657e-4b98-a036-2cb3214e5de1%2F5.png?table=block&id=6cfe5684-5d99-4069-95f7-49cfb03850e6&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1730&userId=&cache=v2)
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


프로그래머스 알고리즘 문제 응용 - 푸드 파이트 대회  
--
### 문제 설명  

수웅이는 매달 주어진 음식을 빨리 먹는 푸드 파이트 대회를 개최합니다.  
이 대회에서 선수들은 1대 1로 대결하며, 매 대결마다 음식의 종류와 양이 바뀝니다.  
대결은 준비된 음식들을 일렬로 배치한 뒤, 한 선수는 제일 왼쪽에 있는 음식부터 오른쪽으로,
다른 선수는 제일 오른쪽에 있는 음식부터 왼쪽으로 순서대로 먹는 방식으로 진행됩니다.  
중앙에는 물을 배치하고, 물을 먼저 먹는 선수가 승리하게 됩니다.  
  
이때, 대회의 공정성을 위해 두 선수가 먹는 음식의 종류와 양이 같아야 하며, 음식을 먹는 순서도 같아야 합니다.  
또한, 이번 대회부터는 칼로리가 낮은 음식을 먼저 먹을 수 있게 배치하여 선수들이 음식을 더 잘 먹을 수 있게 하려고 합니다.  
이번 대회를 위해 수웅이는 음식을 주문했는데, 대회의 조건을 고려하지 않고 음식을 주문하여 몇 개의 음식은 대회에 사용하지 못하게 되었습니다.  
  
예를 들어, 3가지의 음식이 준비되어 있으며, 칼로리가 적은 순서대로 1번 음식을 3개, 2번 음식을 4개, 3번 음식을 6개 준비했으며, 물을 편의상 0번 음식이라고 칭한다면,  
두 선수는 1번 음식 1개, 2번 음식 2개, 3번 음식 3개씩을 먹게 되므로 음식의 배치는 "1223330333221"이 됩니다. 

따라서 1번 음식 1개는 대회에 사용하지 못합니다.  
  
수웅이가 준비한 음식의 양을 칼로리가 적은 순서대로 나타내는 정수 배열 food가 주어졌을 때,  
대회를 위한 음식의 배치를 나타내는 문자열을 return 하는 solution 함수를 완성해주세요.  
  
제한사항  
2 ≤ food의 길이 ≤ 9  
1 ≤ food의 각 원소 ≤ 1,000  
food에는 칼로리가 적은 순서대로 음식의 양이 담겨 있습니다.  
food[i]는 i번 음식의 수입니다.  
food[0]은 수웅이가 준비한 물의 양이며, 항상 1입니다.  
정답의 길이가 3 이상인 경우만 입력으로 주어집니다.  
  
입출력 예  
food result  
[1, 3, 4, 6] "1223330333221"  
[1, 7, 1, 2] "111303111"  

입출력 예 설명  
  
입출력 예 #1  
문제 예시와 같습니다.  
  
입출력 예 #2  
두 선수는 1번 음식 3개, 3번 음식 1개를 먹게 되므로 음식의 배치는 "111303111"입니다.

### 문제풀이

```csharp
using System.Text;
using System;

using System;
using System.Text;

public class Solution 
{
    public string solution(int[] food)
    {
	    // StringBuilder 타입의 왼쪽 오른쪽 변수를 선언
        StringBuilder leftTable = new StringBuilder();
        StringBuilder rightTable = new StringBuilder();

		// 준비된 음식 배열을 반복
		// 배열을 물은 건너 뛰어야 하기 때문에 -1 부터 시작하여 배열을 반복 
        for (int i = food.Length - 1; i >= 1; i--)
        {
		    // 왼쪽 오른쪽 테이블의 최대 갯수
            int half = food[i] / 2;

			// 음식의 절반을 왼쪽과 오른쪽 테이블에 추가
            for (int j = 0; j < half; j++)
            {
	            // 왼쪽의 경우 i가 반복될 때 마다 0번 인덱스에 문자가 추가됨
	            // 뒤에서 앞으로 추가되는 형태로 나타남
	            // 3 > 13 > 113
                leftTable.Insert(0,i);
                // 오른쪽은 출력 되는 데로 추가 
                // 이렇게 하면 서로 대칭되는 형태가 됨
				// 3 > 31 > 311 
                rightTable.Append(i);
            }
        }
        return leftTable + "0" + rightTable;
    }
}
```
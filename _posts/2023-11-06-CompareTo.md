---
layout: post
title: 문자열 내 마음대로 정렬하기 (Array, CompareTo)
subtitle: 배열 정렬과 비교로 알고리즘 해결하기
author: Daniel
categories: Algorithm
tags: 
 - Algorithm
 - Programming
 - Csharp
banner:
 image : https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

## Array Sort

프로그래머스 문제 중

문자열 내 마음대로 정렬하기 문제 관련 Array.Sort와 CompareTo 메소드를 이용한 문자열 정렬에 관한 내용

---

### Array.Sort 메서드

`Array.Sort`는 .NET 프레임워크에서 제공하는 다재다능한 메서드로, 단일 차원 배열 내의 요소들을 정렬합니다. 기본 정렬부터 개발자가 정의한 맞춤 정렬 로직에 이르기까지 다양한 정렬 요구 사항에 대응하기 위한 여러 오버로드를 제공합니다.

#### 기본 정렬

가장 간단한 형태의 정렬은 `Array.Sort`를 호출하고 정렬할 배열을 전달함으로써 이루어집니다:

```csharp
int[] numbers = { 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5 };
Array.Sort(numbers);
// 'numbers' 배열은 이제 정렬되었습니다.
```

#### 비교를 통한 정렬

더 복잡한 정렬 기준을 위해 `Array.Sort`는 `Comparison<T>`와 같은 대리자나 `IComparer`와 같은 인터페이스와 함께 사용될 수 있습니다. 이는 사용자 정의 객체를 정렬하거나 특정 정렬 규칙이 있을 때 특히 유용합니다.

---

### CompareTo 메서드

`CompareTo` 메서드는 동일한 타입의 다른 객체와 현재 객체를 비교할 수 있게 하는 `IComparable` 인터페이스의 일부입니다.

#### 정렬에서의 역할

정렬 시 `Array.Sort`는 요소들의 순서를 결정하기 위해 `CompareTo` 메서드에 의존합니다. 각 요소의 `CompareTo` 메서드는 다른 요소를 매개변수로 호출되어 그들 사이의 상대적 순서를 결정합니다.

#### 맞춤형 CompareTo

`IComparable`을 구현하는 사용자 정의 클래스의 객체 배열을 정렬할 경우, 자신만의 `CompareTo` 메서드를 정의할 수 있습니다:

```csharp
public class Person : IComparable<Person>
{
    public string Name { get; set; }
    public int Age { get; set; }

    public int CompareTo(Person other)
    {
        // 나이를 기준으로 정렬
        return this.Age.CompareTo(other.Age);
    }
}
```

이 구현을 사용하면 `Person` 객체의 배열을 나이에 따라 정렬할 수 있습니다.

---

### Array.Sort와 CompareTo의 실제 사용

특정 위치에 있는 문자를 기준으로 문자열 배열을 정렬해야 하는 상황을 생각해보세요:

```csharp
string[] strings = { "apple", "banana", "cherry" };
Array.Sort(strings, (x, y) => x[1].CompareTo(y[1]));
// 문자열은 이제 두 번째 문자를 기준으로 정렬됩니다.
```

여기서 람다 표현식을 사용하여 `Array.Sort`에 대한 비교 로직을 정의하는데, 각 문자열의 두 번째 문자를 비교합니다.

#### 동점 처리

비교하는 문자가 동일한 경우, 문자열을 알파벳 순으로 정렬하고 싶을 수 있습니다. 비교 로직을 확장하여 이를 처리할 수 있습니다:

```csharp
Array.Sort(strings, (x, y) =>
{
    int primary = x[1].CompareTo(y[1]);
    if (primary == 0)
    {
        return x.CompareTo(y); // 두 번째 문자가 같을 경우 알파벳 순으로 정렬
    }
    return primary;
});
```

---

문제
--

## 문자열 내 마음대로 정렬하기
##### 문제 설명

문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 ["sun", "bed", "car"]이고 n이 1이면 각 단어의 인덱스 1의 문자 "u", "e", "a"로 strings를 정렬합니다.

##### 제한 조건

- strings는 길이 1 이상, 50이하인 배열입니다.
- strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
- strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
- 모든 strings의 원소의 길이는 n보다 큽니다.
- 인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

##### 입출력 예

|strings|n|return|
|---|---|---|
|["sun", "bed", "car"]|1|["car", "bed", "sun"]|
|["abce", "abcd", "cdx"]|2|["abcd", "abce", "cdx"]|

##### 입출력 예 설명

**입출력 예 1**  
"sun", "bed", "car"의 1번째 인덱스 값은 각각 "u", "e", "a" 입니다. 이를 기준으로 strings를 정렬하면 ["car", "bed", "sun"] 입니다.

**입출력 예 2**  
"abce"와 "abcd", "cdx"의 2번째 인덱스 값은 "c", "c", "x"입니다. 따라서 정렬 후에는 "cdx"가 가장 뒤에 위치합니다. "abce"와 "abcd"는 사전순으로 정렬하면 "abcd"가 우선하므로, 답은 ["abcd", "abce", "cdx"] 입니다.

## 풀이

```csharp
using System;
public class Solution {
    public string[] solution(string[] strings, int n) 
    {
        Array.Sort(strings, (x, y) => {
            int sortString = x[n].CompareTo(y[n]);
            return  sortString == 0 ? string.Compare(x, y) : sortString;
        });
        return strings;
    }
}
```

1. 1차 적으로 `Array.Sort()`를 이용해서 strings 문자열 배열을 오름차순으로 정렬

```csharp
Array.Sort(strings, (x,y) => x[n].CompareTo(y[n]));
```

2. 다음으로 인덱스의 문자가 같다면, 전체 문자열을 오름차순으로 정렬

```csharp
Array.Sort(strings, (x,y) => {
	int sortString = x[n].CompareTo(y[n]);
	return sortString == 0 ? string.Compare(x,y) : sortString;
})
```

- CompareTo의 경우 비교 되는 두 객체의 상대적 순서를 나타내는 정수를 반환함

>- 반환 값이 **0보다 작으면** <br>첫 번째 객체가 두 번째 객체보다 작다는 것을 나타냅니다 <br>(즉, 첫 번째 객체가 두 번째 객체보다 앞에 오게 됩니다).
>
>- 반환 값이 **0이면**<br> 두 객체가 같다는 것을 나타냅니다 (정렬 순서에서 위치가 변경되지 않습니다).
>
>- 반환 값이 **0보다 크면**<br> 첫 번째 객체가 두 번째 객체보다 크다는 것을 나타냅니다 <br>(즉, 첫 번째 객체가 두 번째 객체보다 뒤에 오게 됩니다).
---
layout: post
title: 알고리즘 결괏값 확인 자동화
subtitle: Reflection을 이용한 실행 프로그래밍
author: Daniel
categories: C#
tags: 
 - Csharp
 - Algorithm
banner:
  image: https://i.imgur.com/MKFzAXF.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

코드실행 자동화
--
- 기존에 알고리즘을 실행하기 위해서는 Program.cs의 Main 함수를 실행하는 방식
- 해당 Solution을 실행 하기 위해서는 Main 안에서 Class.solution을 실행 해야하는 귀찮음
- 별도의 Code 수정 없이 실행 버튼을 누르면 자동으로 실행되는 로직을 구상

### Program.cs

```csharp
using System.Reflection;

public abstract class KataBase
{
    public abstract void Example();
}

public static class Program
{
    public static void Main()
    {
        Console.WriteLine("실행하고자 하는 문제의 Class Name을 적어주세요:");
        string problemClassName = Console.ReadLine();

        var testType = Assembly.GetExecutingAssembly().GetTypes()
            .FirstOrDefault(t => t.IsSubclassOf(typeof(KataBase)) && t.Name == problemClassName);

        if (testType != null)
        {
            var instance = Activator.CreateInstance(testType) as KataBase;
            instance?.Example(); 
        }
        else
        {
            Console.WriteLine($"Problem class '{problemClassName}' not found.");
        }
    }
}
```

- 먼저 Reflection에 대해서 알아야 합니다.
### Reflection

- `Reflection`은 프로그램 자체 구조와 동작을 검사하고 조작할 수 있게 해주는 기능
- Assembly, Module 및 Type에 대한 정보뿐만 아니라 Type의 인스턴스를 동적생성하며
- 메서드에 바인딩, Field, Attribute에 엑세스 하는 기능을 포함합니다.

#### Inspecting Type Information (유형 정보 검사)

**Type Class**
- `System.Type` 은 Reflection의 핵심 
- 클래스, 인터페이스, 배열, 값, 열거형, 파라매터, 제네릭, 퍼블릭, 프라이빗 생성 제네릭 유형 등 을 나타냄

**Type 정보 가져오기**
- `GetType()`메서드를 이용해서 객체의 `Type`인스턴스를 가져오거나, `typeof` 키워드를 사용하여 Type의 인스턴스를 가져올 수 있습니다.
- `Type Instance`를 가져오면, 메서드, 속성, 필드 이벤트등 다양한 측면을 검사할 수 있습니다.

#### Dynamic Instance Creation (동적 인스턴스 생성)

**인스턴스 생성**
- `Reflection`은 `Compile` 시 정확한 유형을 알지 못해도 `RunTime`에 인스턴스를 생성하는 기능을 제공
- `Acivator.CreateIntance`는 해당 목적으로 사용되는 방법
- 플러그인 아키텍쳐와 같은 Scenario, Compile Time에 알 수 없는 유형을 처리할 때 유용함
- `Type`의 이름만 알고 있을 때 유용

#### Members Invocation and Access (맴버 호출 및 접근)

**Member Access**
- `Type`객체가 있으면, `GetMethod`, `GetProperty`, `GetField`, `GetEvent`등을 이용하여 `Type Member`에 접근 할 수 있습니다.
- 이후 속성값에 대한 설정 및 메서드를 호출하는 등 조작이 가능해집니다.

**Binding Flag**
- `Reflection Method`에서는 일반적으로 `BindingFlags`를 `overlode`허용하며, 이것은 `Member` 및 `Type`이 수행되는 방식을 제어합니다.

#### Assembly(어셈블리)

**Assembly Class**
- `Assembly` Class를 사용하면, Load 하고 조작할 수 있습니다.

#### Performance Considerations
- `Reflection`의 경우 동적 특성으로 인해서 직접 코드 실행에 비래 속도가 늦을 수 있습니다.
- 성능이 중요한 경우 사용을 지양합니다.

#### 코드에서의 Reflection 사용

```csharp
using System.Reflection;

public static void Main()
{
	// 실행중인 Assembly중에서 KataBase의 클래스 중 입력한 ClassName과 동일한 타입을 찾습니다.
	var testType = Assembly
	.GetExecutingAssembly()
	.GetTypes()
	.FirstOrDefault(t => t.IsSubclassOf(typeof(KataBase)) 
	&& t.Name == problemClassName); 
}
```

### Example Code

```csharp
public class PairNumber : KataBase  
{                                                                                                                                
public string solution(string X, string Y)  
    {        
	    // 기본 반환값을 설정합니다. 이 값은 공통된 숫자가 없을 경우 반환됩니다.  
        string answer = "-1";  
  
        // X의 각 문자에 대한 카운트를 계산합니다.  
        var xCharCounts = X.GroupBy(c => c).ToDictionary(g => g.Key, g => g.Count());  
        // Y의 각 문자에 대한 카운트를 계산합니다.  
        var yCharCounts = Y.GroupBy(c => c).ToDictionary(g => g.Key, g => g.Count());  
  
        // 두 문자열에서 공통으로 나타나는 문자를 저장할 리스트를 생성합니다.  
        var commonChars = new List<char>();  
  
        // X에 있는 모든 문자를 반복하여 확인합니다.  
        foreach (var xChar in xCharCounts)  
        {            
	        // Y에도 같은 문자가 있는지 확인하고, 그렇지 않으면 계속합니다.  
            if (!yCharCounts.TryGetValue(xChar.Key, out int yCount)) continue;  
            // X와 Y에서의 해당 문자의 최소 등장 횟수를 찾습니다.  
            int minCount = Math.Min(xChar.Value, yCount);  
            // 공통 문자를 최소 등장 횟수만큼 리스트에 추가합니다.  
            commonChars.AddRange(Enumerable.Repeat(xChar.Key, minCount));  
        }  
        // 공통 문자가 없는 경우, 기본 반환값을 반환합니다.  
        if (!commonChars.Any()) return answer;  
  
        // 공통 문자를 내림차순으로 정렬합니다.  
        var sortedCommonChars = commonChars.OrderByDescending(c => c).ToArray();  
        // 만약 모든 공통 문자가 '0'이라면, 단일 '0'을 반환합니다.  
        return sortedCommonChars.All(c => c == '0') ? "0" : new string(sortedCommonChars);  
    }
}

public override void Example()  
{  
    string x = "100";  
    string y = "203045";  
    var result = solution(x,y);  
    Console.WriteLine($"결괏값 : {result}"); 
}
```

### 프로그램 실행

- Example 메서드에 예제 입력
- 실행 하려는 Class 실행 (여기서는 "PairNumber")
- 결괏값 확인

![](https://i.imgur.com/MKFzAXF.gif)

마치며
--
별 것 아닌 프로그램 이지만, Reflection에 대해 알 수 있었고, 불편한 것은 편하게 만드는 것이 개발자로서 할 일이라고 생각합니다.
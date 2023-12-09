---
layout: post
title : Clean 코드를 사용하자
subtitle: Clean Code 5가지 
author: Daniel
categories: Programming
tags: 
 - Projramming
banner:
  image: https://i.imgur.com/Kucs3DV.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Clean Code
--
- 프로그래밍에 있어서 이제는 그냥 구현에 만족하는 것이 아니라, 패턴, 변수, 컨벤션등을 지키며 협업을 위한 읽기 좋은 코드 깔끔한 코드를 만들 때이기 때문에 Clean 코드에 대해서 알아보려고 합니다.

### 기이한 이름 (Mysterious Name)

![](https://i.imgur.com/Kucs3DV.jpg)

- 명확한 변수명은 매우 중요합니다.
- 모호한 이른은 코드 자체의 가독성을 낮추고 코드의 이해를 방해함
- 변수, 함수, 클래스, 모듈명 등 이름만 보아도 무엇을 하는지 또는 무엇을 뜻 하는지 알 수 있는 이름이 좋은 이름입니다.
- 오히려 코드를 작성 중 해당 부분에 대한 명확한 이름이 떠오르지 않는다면 설계가 잘못 되었을 수 있다는 것을 명심해야 합니다.

#### 불분명한 변수명 사용

```csharp
public class Data
{
	public DateTime dob {get; set;}
	public string x {get; set;}
	public string y {get; set;}

	public int Calc ()
	{
		DateTime n = DataTime.Now;
		int a = n.Year - dob.Year;

		if (n < dob.AddYears(a))
		{
			a--;
		}
		return a;
	}
}
```

#### 분명한 변수명 사용

```csharp
class Customer
{
	public DateTime DataOfBirth {get;set;}
	public string FirstName {get; set;}
	public string LastName {get; set;}

	public int CalculateAge()
	{
		DateTime now = DateTi,e.Now;
		int age = now.Year - DateOfBirth.Year;

		if (now < DateOfBirth.AddYears(age))
		{
			age--;
		}

		return age;
	}
}
```

### 중복 코드 (Duplicated Code)

- `Don't Repeat Yourself` (DRY)의 원칙
- 똑같은구조의 반복은 최악
- 하나의 클래스 안에 비슷한 함수가 있다면, 중복되는 부분을 함수로 추출
- 혹은 비슷한 함수가 있다면, 공통된 부분을 부모의 함수로 만들고, 자식 함수로 각자 호출하는 방법을 사용
- **`중복된 코드를 하나로 통합하는 것은`** 리팩토링의 가장 기본

#### 중복 연산

- 데이터 로드 및 처리 등 중복된 연산을 실행

```csharp
class ReportGenerator  
{  
    public string GenerateSalesReport()  
    {        
	    // 데이터 로드  
        var data = LoadSalesData();  
        var report = "Sales Report\n";  
        // 데이터 처리  
        foreach (var item in data)  
        {            
	        report += $"Date: {item.Date}, Sales: {item.Amount}\n";  
        }  
        // 리포트 반환  
        return report;  
    }  
    
    public string GenerateEmployeeReport()  
    {        
	    // 데이터 로드  
        var data = LoadEmployeeData();  
        var report = "Employee Report\n";  
        // 데이터 처리  
        foreach (var item in data)  
        {
			report += $"Name: {item.Name}, Role: {item.Role}\n";  
        }  
        // 리포트 반환  
        return report;  
    }
}
```

#### 중복 코드 메서드 화

```csharp
class ReportGenerator  
{  
    // 추출된 범용 함수  
    public string GenerateReport<T>(string title, IEnumerable<T> data, Func<T, string> formatFunc)  
    {        
	    var report = $"{title}\n";  
        foreach (var item in data)  
        {            
	        report += formatFunc(item);        
		}        
		return report;  
    }  
    
    public string GenerateSalesReport()  
    {        
	    var data = LoadSalesData();  
        return GenerateReport("Sales Report", data, item => $"Date: {item.Date}, Sales: {item.Amount}");  
    }  
    public string GenerateEmployeeReport()  
    {        
	    var data = LoadEmployeeData();  
        return GenerateReport("Employee Report", data, item => $"Name: {item.Name}, Role: {item.Role}");  
    }
}
```


### 긴 함수 (Long Function)

![](https://i.imgur.com/C0HXc9u.jpg)

- 짧은 함수는 재사용성도 좋고, 코드를 이해하고 공유하기 쉬워짐
- 하나의 함수에서 모든 기능을 처리하는 것이 아닌 하나의 함수는 하나의 기능만을 수행
- `짧은 함수와 좋은 이름의 조합이 최고`

#### 긴 함수

```csharp
class ReportProcessor  
{  
    public void ProcessReport(string filePath)  
    {        // 파일 읽기  
        var lines = File.ReadAllLines(filePath);  
  
        // 데이터 처리  
        var reportData = new List<ReportData>();  
        foreach (var line in lines)  
        {            
	        var elements = line.Split(',');  
            if (elements.Length == 3)  
            {                
	            var reportItem = new ReportData  
                {  
                    Date = DateTime.Parse(elements[0]),  
                    Category = elements[1],  
                    Amount = decimal.Parse(elements[2])  
                };                
                reportData.Add(reportItem);  
            }        
		}  
        // 데이터 분석  
        var totalAmount = reportData.Sum(item => item.Amount);  
  
        // 결과 출력  
        Console.WriteLine($"Total Amount: {totalAmount}");  
    }
}
```

#### 짧은 함수

- 실제 코드의 전체 라인은 증가 했지만, 각 함수는 하나의 기능을 수행하며
- 실제 메서드의 재사용성 자체가 높아짐

```csharp
class ReportProcessor  
{  
    public void ProcessReport(string filePath)  
    {        
	    var reportData = ReadAndParseFile(filePath);  
        var totalAmount = CalculateTotalAmount(reportData);  
        PrintResult(totalAmount);  
    }  
    
    private List<ReportData> ReadAndParseFile(string filePath)  
    {        
	    var lines = File.ReadAllLines(filePath);  
        var reportData = new List<ReportData>();  
  
        foreach (var line in lines)  
        {            
	        var reportItem = ParseLine(line);  
            if (reportItem != null)  
            {                
	            reportData.Add(reportItem);  
            }        
		}  
        return reportData;  
    }
      
    private ReportData ParseLine(string line)  
    {  
	    var elements = line.Split(',');  
        if (elements.Length == 3)  
        {            
	        return new ReportData  
            {  
                Date = DateTime.Parse(elements[0]),  
                Category = elements[1],  
                Amount = decimal.Parse(elements[2])  
            };        
		}        
		return null;  
    }  
    
    private decimal CalculateTotalAmount(List<ReportData> reportData)  
    {        
	    return reportData.Sum(item => item.Amount);  
    }  
    
    private void PrintResult(decimal totalAmount)  
    {        
	    Console.WriteLine($"Total Amount: {totalAmount}");  
    }
}
```


### 전역 변수의 남용 (Global Variables)

![](https://i.imgur.com/KFIvIuN.jpg)

- 전역 데이터의 사용은 프로그램의 악취 중 가장 독한 악취 중의 하나
- 전역 변수는 어디서나 변경이 가능해서 변경 시점의 추적 및 디버깅을 어렵게 하는 버그의 원인이 됩니다
- 전역 데이터를 써야한다면 const, readonly 등 변경이 불가능 하도록 해야 합니다.
- 클래스 전연 데이터도 변수를 캡슐화 하는 습관이 필요 합니다.
- Singleton의 경우 가장 유명한 전역 변수이지만, 이를 캡슐화 하여 피하도록 합니다.

#### BadCase

```csharp
static class GlobalData  
{  
    public static int GlobalCounter;  
}  
  
class Processor  
{  
    public void Process()  
    {        
	    GlobalData.GlobalCounter++; // 전역 데이터 변경  
        // 다른 처리 로직    
	}  
}  
  
class AnotherProcessor  
{  
    public void AnotherProcess()  
    {        
	    GlobalData.GlobalCounter++; // 동일한 전역 데이터 변경  
        // 다른 처리 로직    
	}  
}
```

#### Good Case

```csharp
class GlobalCounter  
{  
    private int _counter = 0;  
  
    public void Increment()  
    {        
	    _counter++;  
    }  
    
    public int GetCounter()  
    {        
	    return _counter;  
	}
}  
  
class Processor  
{  
    private readonly GlobalCounter _globalCounter;  
  
    public Processor(GlobalCounter globalCounter)  
    {        
	    _globalCounter = globalCounter;  
    }  
    
    public void Process()  
    {        
	    _globalCounter.Increment();  
    }
}  
  
class AnotherProcessor  
{  
    private readonly GlobalCounter _globalCounter;  
  
    public AnotherProcessor(GlobalCounter globalCounter)  
    {    
	    _globalCounter = globalCounter;  
    }  
    
    public void AnotherProcess()  
    {        
	    _globalCounter.Increment();  
    }
}
```


### 주석의 남용

![](https://i.imgur.com/rTYkxrd.jpg)

- 올바른 주석은 아주 좋음
- 그러나 코드만으로 명확하게 이해 되는 것이 더 중요
- 주석은 코드를 변명하기 위한 장치에 가까움
- 즉, 주석이 필요없는 코드로 바꾸는게 우선

#### BadCase

```csharp
public class Calculator  
{  
    // 이 메소드는 두 수를 더합니다.  
    // a는 첫 번째 숫자입니다.    
    // b는 두 번째 숫자입니다.    
    // 결과는 두 숫자의 합입니다.    
    public int Add(int a, int b)  
    {        
	    // a와 b를 더한 값을 반환합니다.  
        return a + b;  
    }
}
```

#### Good Case

```csharp
public class NetworkUtils  
{  
    // TCP 소켓 연결에서 타임아웃을 확인하는 메서드  
    // timeoutInMilliseconds는 타임아웃 시간을 밀리초 단위로 설정합니다.    
    // 소켓 연결이 타임아웃 시간 내에 이루어지지 않으면 예외를 발생시킵니다.    
    
    public void CheckConnectionTimeout(Socket socket, int timeoutInMilliseconds)  
    {        
	    // 소켓 연결이 설정된 시간 내에 이루어지는지 확인하는 복잡한 로직  
        // ...    
	}  
}
```
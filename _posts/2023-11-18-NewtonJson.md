---
layout: post
title: Json 다루기
subtitle: Newtonsoft.Json 다루기
author: Daniel
categories: C#
tags: 
 - Csharp
 - Pfoject
 - Json
 - Programming
banner:
  image:
---

직렬화 역직렬화
--
### JSON 개념

- **JSON** (JavaScript Object Notation)은 사람이 쉽게 읽고 쓸 수 있는 경량 데이터 교환 형식입니다. 
- 기계가 구문 분석하고 생성하기 쉽기 때문에 데이터 전송에 널리 사용됩니다.

|개념|설명|예|
|:--|:--|:--|
|JSON Object|Key / Value Pairs|{"name":"John", "age":30}|
|JSON Array|순서가 지정된 값 목록|{"color":["Red", "Green", "Blue"]}|
|Parsing JSON|JSON 문자열을 사용 가능한<br>데이터 구조로 변환|var obj = JSON.parse(<br>`{"name":"John"}, "age":30`<br>);|
|Stringifying JSON|데이터 구조를 JSON 문자열로 변환|var json = JSON.stringify(<br>{name : "John", age : 30}<br>);|


- **직렬화** 및 **역직렬화**는 컴퓨터 과학의 기본 개념으로, 특히 데이터 저장 및 전송 맥락에서 중요합니다.

#### 직렬화 Serialize

**정의**
- 직렬화는 객체나 데이터 구조를 쉽게 저장하거나 전송할 수 있는 형식으로 변환하는 프로세스입니다. 
- JSON의 컨텍스트에서 직렬화에는 개체(예: C# 클래스의 인스턴스)를 JSON 문자열로 변환하는 작업이 포함됩니다.
    
**사용 사례**
- 직렬화는 다음과 같은 경우에 사용됩니다.    
    - 객체의 상태를 파일이나 데이터베이스에 저장합니다.
    - 네트워크를 통해 객체를 보냅니다(예: 웹 API 또는 원격 프로시저 호출).
    - 웹 애플리케이션에서 세션 상태를 저장하고 검색합니다.

#### 역직렬화 Deserialize

**정의**
- 역직렬화는 직렬화의 역과정입니다. 
- 여기에는 JSON 문자열과 같은 데이터 형식을 다시 개체나 데이터 구조로 변환하는 작업이 포함됩니다. 
- JSON의 컨텍스트에서 역직렬화는 JSON 문자열을 다시 C# 개체로 변환합니다.
    
**사용 사례**
- 역직렬화는 다음과 같은 경우에 사용됩니다. 
    - 파일이나 데이터베이스에서 개체 상태를 검색합니다.
    - 네트워크를 통해 전송되는 데이터를 수신하고 사용합니다.
    - 웹 응용 프로그램에서 세션 상태를 복원합니다.



Newtonsoft.Json 다루기
--
- Bug List의 Json 파일

```json
[
	{  
		"bugName": "Incorrect Value Assignment",  
		"bugDesc": "A variable is assigned a value outside its expected range, causing logic errors.",  
		"bugSolution": "ModifyVariables",  
		"bugDifficulty": 7,  
		"bugComplexity": 6  
	},  
	{  
		"bugName": "Uninitialized Variable",  
		"bugDesc": "A variable is used without being initialized, leading to unpredictable behavior.",  
		"bugSolution": "ModifyVariables",  
		"bugDifficulty": 5,  
		"bugComplexity": 4  
	}
]
```

- Json 을 역직렬화 하여 사용할 수 있는 데이터로 변경

```csharp
# region BugList  
/// <summary>  
/// 버그 목록을 역직렬화 하여 언제든지 사용가능하도록  
/// 리스트에 담는 메서드  
/// </summary>  
private static void ListOfBug()  
{  
	using StreamReader json = new(BugFilePath, Encoding.UTF8);  
	string file = json.ReadToEnd();  
	_bugList = JsonConvert.DeserializeObject<List<Bug>>(file);  
}  
# endregion
```

### .NET StreamReder / json.ReadToEnd

- .NET에서 JSON을 사용하는 경우 파일이나 스트림에서 JSON 데이터를 읽는 데 <br>`StreamReader`와 `ReadToEnd` 메서드가 함께 사용되는 경우가 많습니다.

#### StreamReader

**목적**
- `StreamReader`는 특정 인코딩(예: UTF-8)의 바이트 스트림(예: 파일 또는 네트워크 스트림)에서 문자를 읽는 데 사용되는 .NET Framework의 클래스입니다.
**용도**
- 파일에서 텍스트 데이터를 읽는 데 자주 사용됩니다. 
- 예를 들어 .NET 애플리케이션에서 JSON 파일을 읽는 것입니다.

#### json.ReadTOEnd

**메소드**
- `ReadToEnd`는 현재 위치부터 스트림 끝까지의 모든 문자를 읽어서 단일 문자열로 반환 `StreamReader`의 메소드입니다.
**JSON 컨텍스트에서의 사용법**
- JSON 파일이 있는 경우 `StreamReader`를 사용하여 파일을 열고 `ReadToEnd`를 사용하여 전체 JSON 콘텐츠를 문자열로 읽을 수 있습니다. 
- 그런 다음 Newtonsoft.Json과 같은 JSON 라이브러리를 사용하여 이 문자열을 C# 개체로 역직렬화할 수 있습니다.



Newtonsoft.json
--

>Json.NET으로도 알려진 Newtonsoft.Json은 널리 사용되는 .NET용 고성능 JSON 프레임워크입니다. .NET 애플리케이션에서 JSON 데이터 작업에 널리 사용되며, JSON 데이터 직렬화 및 역직렬화,<br> LINQ to JSON 및 기타 다양한 JSON 관련 작업을 위한 기능을 제공합니다. <br>다음은 몇 가지 주요 기능과 사용 방법에 대한 개요입니다.

#### 설치 
- NuGet Package Manager를 통해서 설치 가능하다

```sh
Install-Package Newtonsoft.Json
```

#### 직렬화 SerializeObject
- 직렬화는 객체를 JSON 문자열로 변환하는 프로세스

```csharp
var product = new Product 
{ 
	Name = "Apple", 
	Price = 1.99, 
	ExpiryDate = new DateTime(2023, 1, 24) 
}; 

string json = JsonConvert.SerializeObject(product, Formatting.Indented);
```

#### 역직렬화 DeserializeObjet
- JSON 문자열을 다시 객체로 변환하는 프로세스

```csharp
string json = @"{
	'Name': 'Apple', 
	'Price': 1.99, 
	'ExpiryDate': '2023-01-24T00:00:00' 
}"; 

Product product = JsonConvert.DeserializeObject<Product>(json);
```

#### LINQ => JSON
- LINQ to JSON은 JSON 문서를 동적으로 쿼리하고 조작하는 방법을 제공합니다
- 런타임까지 JSON 데이터의 구조를 알 수 없는 상황에 유용합니다.

```csharp
string json = @"{
	'Name': 'Apple', 
	'Price': 1.99 
}"; 

JObject obj = JObject.Parse(json); 
string name = (string)obj["Name"]; 
// name == "Apple"
```

#### Null 값 처리
- 직렬화 및 역직렬화 중에 null 값이 처리되는 방법을 제어할 수 있습니다.
- 예를 들어 직렬화할 때 null 값을 무시하려면 다음을 수행합니다.

```csharp
var settings = new JsonSerializerSettings 
{ 
	NullValueHandling = NullValueHandling.Ignore 
}; 

string json = JsonConvert.SerializeObject(product, settings);
```


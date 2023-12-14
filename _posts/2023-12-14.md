---
layout: post
title: Strategy Pattern을 이용한 데이터 로드 리팩토링
subtitle: 전략 패턴과 인터페이스를 사용한 Switch 없는 코드 리팩토링
author: Daniel
categories: Unity
tags: 
 - Csharp
 - Unity
 - Programming
banner:
  image: https://i.imgur.com/Vjgm1NQ.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Strategy Pattern (전략패턴)
--

![](https://i.imgur.com/Vjgm1NQ.jpg)

> 다양한 클래스 내에 알고리즘을 캡슐화하여 객체가 런타임에 동작할 수 있도록 하는 객체지향프로그래밍의 `동작 디자인 패턴`입니다.

> 이 패턴은 Context에서 서로 다른 알고리즘이나 Procedure간에 동적으로 전환해야하는 상황에 특히 유용합니다.

### 전략 인터페이스 (Strategy Pattern)

- 알고리즘 계열에 대한 공통 Interface를 만드는 것이 포함되는 단계
- 자체 클래스에 캡슐화 된 각 알고리즘은 이 인터페이스를 구현 함
- 이 인터페이스는 알고리즘을 실행가기 위해 호출되는 메서드를 정의

### 구체적인 전략 (Concrete Strategies)

- 전략 인터페이스를 구현하는 클래스
- 각 클래스는 특정 알고리즘이나 동작을 캠술화
- 접근 방식 및 알고리즘 수에 따라 달라질 수 있음

### 컨텍스트 클래스 (Context Class)

- 전략을 사용하는 클래스
- 이 클래스는 전략 객체에 대한 참조가 포함 되어 있음
- 구체적인 전략 개체로 구성할 수 있음
- 컨텍스트는 어떤 알고리즘이 실행되고 있는지 알 수 없음
- 단지 전략 메소드를 실행하는 방법만 알고 있음

### 클라이언트 (Client)

- 클라이언트는 컨텍스트 클래스에 원하는 전략을 설정
- 그런 다음 컨텍스트는 수명 전반에 걸쳐 선택한 전략을 사용하거나 필요에 따라 전략을 변경 가능

### 주요 이점

#### 유연성 및 재사용성

- 런타임에 전략을 전환하여 유연성을 제공
- 또한 전략은 격리되어 있으므로 다양한 상황에서 재사용 할 수 있음

#### 디커플링

- 전략 패턴은 알고리즘을 컨텍스트 클래스에서 분리하는 데 도움이 되어 유지 관리 가능성이 향상되고, 복잡성이 줄어들게 됨

#### 확장 용이성

- 개방형 / 패쇄형 원칙을 준수하며, 새로운 전략을 추가할 때 컨텍스트를 수정할 필요가 없음


코드 리팩토링
--

- 기존 DataParser에서 Csv 파일을 를 각 Loader Interface에 맞게 컨텍스트를 실행하여 Json 파일로 변환 생성하는 기능을 가지고 있습니다.
- Switch 문을 이용하여 각 Data의 종류를 string을 확인하여 각 메서드를 실행하는 로직 입니다.

### 기존 Data

#### 호출부 (CreateJsonFile)

- "string" 값을 전달하여 해당 데이터를 인터페이스와 함께 전달 합니다.

```csharp
public static void CreateJsonFile()  
{  
    CreateData<ItemDataLoader>("ItemData");  
    CreateData<CharacterDataLoader>("CharacterData");  
    CreateData<JobDataLoader>("JobData");  
}
```

#### Json Data 생성부 (CreateData)

- Csv  File Path를 참조하여 해당 데이터를 읽어 드리고 직렬화 하여 Json File Path로 지정된 위치에 Json 파일을 생성합니다.

```csharp
private static void CreateData<T>(string csvFileName) where T : new()  
{  
    T loader = new();  
    string[] lines = File.ReadAllText($"{CsvFilePath}/{csvFileName}.csv").Split("\n");  
    for (int i = 1; i < lines.Length; i++)  
    {        
	    string[] row =  lines[i].Replace("\n", "").Split(',');  
        if (row.Length == 0 || string.IsNullOrEmpty(row[0])) continue;  
        MappingData(csvFileName, row, loader);  
    }    
    string jsonFile = JsonConvert.SerializeObject(loader, Formatting.Indented);  
    File.WriteAllText($"{JsonFilePath}/{csvFileName}.json",jsonFile);  
}
```

#### 컨텍스트 분기 (MappingData)

- 제네릭을 이용하여 각 Loader 타입에 맞는 데이터 변환방식을 사용합니다.
- 하지만 "string" 값을 이용한 switch를 이용하여 각 메서드를 분류 합니다.

```csharp
private static void MappingData<T>(string csvFileName, string[] row, T loader) where T : new()  
{  
    switch (csvFileName)  
    {        
	    case "ItemData":  
            ItemData(row,loader as ItemDataLoader);  
            break;  
        case "JobData":  
            JobData(row, loader as JobDataLoader);  
            break;  
        case "CharacterData":  
            CharacterData(row, loader as CharacterDataLoader);  
            break;  
    }
}
```

#### Data Parser

- 각 메서드 실행에서 row 문자열 배열과 Loader를 매개변수로 해당 데이터 양식에 맞게 데이터를 파싱하여 Json 파일로 직렬화하는데 사용 됩니다.

```csharp
#region ItemData  
private static void ItemData(string[] row, ItemDataLoader loader)  
{   
	var equipPosStrings = row[4].Trim(new char[] { '[', ']' }).Split('/');  
    var equipPosEnums = equipPosStrings.Select(s => (EquipPosition)Enum.Parse(typeof(EquipPosition), s)).ToArray();  
  
    loader.items.Add(new ItemData  
    {  
        PrimeKey = row[0],  
        Type = (Itemtypes)Enum.Parse(typeof(Itemtypes), row[1]),  
        Attribute = int.Parse(row[2]),  
        Price = int.Parse(row[3]),  
        EquipPositions = equipPosEnums // Assigning the converted enum array  
    });  
}  
#endregion  


#region CharacterData  
private static void CharacterData(string[] row, CharacterDataLoader loader)  
{  
    loader.character.Add(new CharacterData  
    {  
        PrimeKey = row[0],  
        Name = row[1],  
        JobClass = (JobClass)Enum.Parse(typeof(JobClass),row[2]),  
        Level = int.Parse(row[3]),   
Damage = int.Parse(row[4]),  
        Defense = int.Parse(row[5]),   
CriticalRate = int.Parse(row[6]),   
Inventory = null  
    });  
}  
#endregion  
  
#region JobData  
private static void JobData(string[] row, JobDataLoader loader)  
{  
    loader.jobs.Add(new JobData  
    {  
        JobClass = (JobClass)Enum.Parse(typeof(JobClass), row[0]),  
        Desc = row[1]  
    });
}  
#endregion
```


#### ILoader (로더 인터페이스)

- 먼저 상속을 하는 부모 인터페이스 입니다.
- 모든 DataLoader 는 해당 인터페이스를 상속 받습니다. 
- 전략 패턴을로 생각한다면 `전략 인터페이스` 부분입니다.

```csharp
public interface ILoader<TKey, TValue>  
{  
    Dictionary<TKey, TValue> CreateData();  
}
```

#### 각 DataType 별 Loader Class

- ILoader를 상속받는 Data Loader 들 입니다.
- 위에서 설명한 전략패턴중 `구체적인 전략` 부분이 될 것 입니다.

```csharp
[Serializable]
public class CharacterDataLoader : ILoader<string, CharacterData>  
{  
    public List<CharacterData> character = new();  
  
    public Dictionary<string, CharacterData> CreateData()  
    {        
	    return character.ToDictionary(player => player.PrimeKey);  
    }
}

[Serializable]
public class ItemDataLoader : ILoader<string, ItemData>  
{  
    public List<ItemData> items = new();  
  
    public Dictionary<string, ItemData> CreateData()  
    {        
	    return items.ToDictionary(item => item.PrimeKey);  
    }
}

[Serializable]
public class JobDataLoader : ILoader<string, JobData>  
{  
    public List<JobData> jobs = new();  
  
    public Dictionary<string, JobData> CreateData()  
    {        
	    return jobs.ToDictionary(job => job.JobClass.ToString());  
    }
}
```


전략패턴을 사용한 코드 리펙토링
--

- 전략 패턴을 사용하여 `전략 인터페이스`, `구체적인 전략`, `컨텍스트 클래스`, `클라이언트` 구조로 로직을 리팩토링 하겠습니다.

### 전략인터페이스 확장

- 기존 ILoader를 확장하여 추가적인 메소드를 구현합니다.

```csharp
public interface ILoader<TKey, TValue>  
{  
    Dictionary<TKey, TValue> CreateData(); 
    
    void MapData(string[] row); 
}
```

#### 🤔 왜 접근 제한제한자 없이 void로 선언하는지?

- 상속이니깐 Protected virtual 선언 해야 하지 않나? 할 수 있습니다.
- C#에서 Interface는 **`public`** 만 가능합니다.
- 인터페이스의 목적은 인터페이스를 구현하는 모든 클래스에서 공개적으로 구현하는 계약을 정의 하기 때문입니다.
- 따라서 Interface에 정의된 모든 메서드를 본질적으로 **`public`** 이며, `protected, private, internal`일 수 없습니다.
- 하지만 메소드가 구현 클래스에서 공개적으로 호출이 가능하지 않고, `ILoader<Tkey, TValue>`를 구현하는 클래스 계층 내에서만 호출 가능하도록 
- 경우에 따라서는 protected를 사용할 수 있지만, 여기서는 사용하지 않습니다.
- 그러므로 우리는 `void` 반환부만 작성하도록 합니다.


### Loader 수정 및 확장 (구체적인 전략)

- 이제 각 Loader에서 `MapData` 메소드를 구현합니다.
- 여기에는 원래 `DataParser`에서 작성 되었던 매핑 로직이 해당 클래스로 이동합니다.

#### ItemDataLoader

```csharp
[Serializable]  
public class ItemDataLoader : ILoader<string, ItemData>  
{  
    public List<ItemData> items = new();  
  
    public Dictionary<string, ItemData> CreateData()  
    {        
	    return items.ToDictionary(item => item.PrimeKey);  
    }  
    void ILoader<string, ItemData>.MapData(string[] row)  
    {        
	    var equipPosStrings = row[4].Trim('[', ']').Split('/');  
        var equipPosEnums = equipPosStrings.Select(s => (EquipPosition)Enum.Parse(typeof(EquipPosition), s)).ToArray();  
  
        items.Add(new ItemData  
        {  
            PrimeKey = row[0],  
            Type = (Itemtypes)Enum.Parse(typeof(Itemtypes), row[1]),  
            Attribute = int.Parse(row[2]),  
            Price = int.Parse(row[3]),  
            EquipPositions = equipPosEnums  
        });    
	}
}
```

#### CharacterDataLoader

```csharp
[Serializable]  
public class CharacterDataLoader : ILoader<string, CharacterData>  
{  
    public List<CharacterData> character = new();  
  
    public Dictionary<string, CharacterData> CreateData()  
    {        
	    return character.ToDictionary(player => player.PrimeKey);  
    }  
    void ILoader<string, CharacterData>.MapData(string[] row)  
    {        
	    character.Add(new CharacterData  
        {  
            PrimeKey = row[0],  
            Name = row[1],  
            JobClass = (JobClass)Enum.Parse(typeof(JobClass),row[2]),  
            Level = int.Parse(row[3]),   
Damage = int.Parse(row[4]),  
            Defense = int.Parse(row[5]),   
CriticalRate = int.Parse(row[6]),   
Inventory = null  
        });  
    }
}
```

#### JobDataLoader

```csharp
[Serializable]  
public class JobDataLoader : ILoader<string, JobData>  
{  
    public List<JobData> jobs = new();  
  
    public Dictionary<string, JobData> CreateData()  
    {        
	    return jobs.ToDictionary(job => job.JobClass.ToString());  
    }  
    void ILoader<string, JobData>.MapData(string[] row)  
    {        
	    jobs.Add(new JobData  
        {  
            JobClass = (JobClass)Enum.Parse(typeof(JobClass), row[0]),  
            Desc = row[1]  
        });    
	}
}
```



### 컨텍스트 클래스

- 기존 `DataParser` 클래스의 데이터 매핑 메서드를 수정합니다.
- switch 분기를 처리하던 부분이 필요 없어지게 되었습니다.

```csharp
private static void MappingData<T, TValue>(string[] row, T loader) where T : ILoader<string, TValue>  
{  
    loader.MapData(row);  
}
```

- `CreateData` 메서드로 이에 맞춰 수정해 줍니다.
- csv파일 이름을 전달하던 부분을 삭제하고, 메서드가 ILoader 인터페이스를 참조 하도록 합니다.

```csharp
private static void CreateData<T, TValue>(string csvFileName) where T : ILoader<string, TValue>, new()  
{  
    T loader = new();  
    string[] lines = File.ReadAllText($"{CsvFilePath}/{csvFileName}.csv").Split("\n");  
    for (int i = 1; i < lines.Length; i++)  
    {        
	    string[] row =  lines[i].Replace("\n", "").Split(',');  
        if (row.Length == 0 || string.IsNullOrEmpty(row[0])) continue;  
        MappingData<T, TValue>(row, loader);  
    }    
    string jsonFile = JsonConvert.SerializeObject(loader, Formatting.Indented);  
    File.WriteAllText($"{JsonFilePath}/{csvFileName}.json",jsonFile);  
}
```

- 그리고 마지막으로 기존 Data를 Json으로 매핑하던 로직을 전부 지워 줍니다.

**삭제 전**

![](https://i.imgur.com/4z4Tz6r.jpg)


**삭제 후** (있었는데 없었습니다.)

![](https://i.imgur.com/aB2PP7K.jpg)



### 클라이언트

- 마지막으로 호출부를 수정해 줍니다.

```csharp
public static void CreateJsonFile()  
{  
    CreateData<ItemDataLoader, ItemData>("ItemData");  
    CreateData<CharacterDataLoader, CharacterData>("CharacterData");  
    CreateData<JobDataLoader, JobData>("JobData");  
}
```

- 추가적으로 `Csv File Name`과 `TValue`의 이름이 같으므로 string 값을 `Reflection`을 이용하여 해당 타입의 이름을 넣어서 수정해 줍니다.

```csharp
public static void CreateJsonFile()  
{  
    CreateData<ItemDataLoader, ItemData>();  
    CreateData<CharacterDataLoader, CharacterData>();  
    CreateData<JobDataLoader, JobData>();  
}  
  
private static void CreateData<T, TValue>() where T : ILoader<string, TValue>, new()  
{  
    T loader = new T();  
    
    string typeName = typeof(TValue).Name;  
    string[] lines = File.ReadAllText($"{CsvFilePath}/{typeName}.csv").Split("\n");  
    
    for (int i = 1; i < lines.Length; i++)  
    {        
	    string[] row =  lines[i].Replace("\n", "").Split(',');  
        if (row.Length == 0 || string.IsNullOrEmpty(row[0])) continue;  
        MappingData<T, TValue>(row, loader);  
    }    
    
    string jsonFile = JsonConvert.SerializeObject(loader, Formatting.Indented);  
    File.WriteAllText($"{JsonFilePath}/{typeName}.json",jsonFile);  
}
```


테스트
--

- 이제 해당 기능이 정상적으로 작동하는지 테스트를 진행합니다.

![](https://i.imgur.com/CKbs0gT.gif)

![](https://i.imgur.com/wl0PfUt.jpg)


- 정상적으로 데이터가 변환되는 것을 확인 하였습니다.



마치며
--

처음 클린코드에서 switch를 사용하지 말라고 하셨을때, "그럼 뭘로 구분하지?" "분기 없이 어떻게 분기를 조절하지?" 라고 생각했는데 전략 패턴을 이용하면 이러한 것들이 사용가능 해진다는 것을 알게 된 중요한 경험이 되었습니다.

앞으로 DataManager를 작성시에 더욱 발전된 로직을 완성할 수 있을 것 입니다.
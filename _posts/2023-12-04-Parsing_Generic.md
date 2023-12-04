---
layout: post
title: Csv Convert to Json Generic
subtitle: 제네릭을 사용한 기능 리팩토링
author: Daniel
categories: Unity
tags: 
 - Unity 
 - Project
 - Programming
banner:
  image: https://d2sofvawe08yqg.cloudfront.net/CSharpGenerics/s_hero2x?1620543883
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Convert Refactoring
--

- CSV Data Paser 기능 중 서비스의 확장성 및 일반화를 위해 Generic <`T`>를 사용하여 범용적으로 사용 가능하도록 리팩토링을 수정합니다.

### Before

```csharp
#if UNITY_EDITOR
    [MenuItem("Tools/ParseCSV")]
    public static void ParseCsv()
    {
        ParseEnemyData("Enemy");
    }

    private static void ParseEnemyData(string csvFileName)
    {
        EnemyDataLoader loader = new();

        string[] lines = File.ReadAllText($"{Application.dataPath}/@CsvData/{csvFileName}Data.csv").Split("\n");

        for (int y = 1; y < lines.Length; y++)
        {
            string[] row = lines[y].Replace("\r", "").Split(',');
            
            if (row.Length == 0 || string.IsNullOrEmpty(row[0])) continue;
            
            EnemyData.FireType enumFire = (EnemyData.FireType)Enum.Parse(typeof(EnemyData.FireType), row[4]);
            
            loader.enemies.Add(new()
            {
                enemyType = (EnemyData.EnemyKey)Enum.Parse(typeof(EnemyData.EnemyKey), row[0]),
                keyName = Enum.Parse(typeof(EnemyData.EnemyKey), row[0]).ToString(),
                speed = float.Parse(row[1]),
                hp = int.Parse(row[2]),
                damage = int.Parse(row[3]),
                fireType = enumFire.ToString()
            });
        }

        string jsonFile = JsonConvert.SerializeObject(loader, Formatting.Indented);
        File.WriteAllText($"{Application.dataPath}/@JsonData/{csvFileName}Data.json", jsonFile);
        AssetDatabase.Refresh();
    }
#endif
```

- 기존 코드의 문제는 단 하나만의 DataLoader 기능만을 수행하며, 단 하나의 타입만을 수행 할 수 있습니다.
- 이는 다른 타입이 추가 되면 해당 코드를 또 다시 작성해야하는 문제가 발생합니다.
- 왜냐하면 데이터 타입은 한가지로 정해져 있어서, 다른 타입을 적용하기 위해서는 또다른 타입을 할당 또는 반환하는 메서드를 작성해야 하는 불편함이 있습니다.

- 이때 사용하는 것이 **Generic** 을 사용하여, 메서드를 일반화하여 어떤 타입이더라도 사용이 가능해집니다.

### Generic

- 제네릭(generics)은 C#을 비롯한 많은 프로그래밍 언어에서 타입(type)의 안전성을 보장하면서 코드의 재사용성을 높이기 위해 사용되는 기능입니다. 
- 제네릭을 사용하면, 특정 타입에 의존하지 않고 다양한 데이터 타입을 처리할 수 있는 범용적인 클래스나 메서드를 작성할 수 있습니다.
- Unity 개발에서 제네릭을 사용하는 대표적인 예로는 다양한 컴포넌트나 데이터 타입을 취급하는 로더(loader)나 매니저(manager) 클래스를 들 수 있습니다. 
- 제네릭을 활용하면, 특정 타입에 대한 참조를 미리 정의하지 않고도, 실행 시에 원하는 타입을 사용하여 클래스나 메서드를 인스턴스화할 수 있습니다.

#### 장점

1. **타입 안전성(Type Safety)** : 제네릭을 사용하면 컴파일 타임에 타입을 체크할 수 있어, 런타임에 발생할 수 있는 타입 관련 오류를 미연에 방지할 수 있습니다.
2. **코드 재사용성(Code Reusability)** : 한 번 작성한 제네릭 클래스나 메서드는 다양한 타입에 대해 재사용할 수 있으므로, 코드의 양을 줄이고 유지보수를 용이하게 합니다.
3. **성능(Performance)** : 박싱(boxing)이나 언박싱(unboxing)과 같은 불필요한 타입 변환을 줄여주어, 성능 향상에 도움이 됩니다.

### After

#### ParseCSV

```csharp
[MenuItem("Tools/ParseCSV")]
public static void ParseCsv()
{
	ParseData<EnemyDataLoader>("Enemy");
	ParseData<SkillDataLoader>("Skill");
	ParseData<StageDataLoader>("Stage");
}
```

#### ParseData<`T`>

```csharp
#region Parsing Method
/// <summary>
///  CSV_Data 파싱 하여 JSON에 덮어 쓰기
/// </summary>
/// <param name="csvFileName"></param>
private static void ParseData<T>(string csvFileName) where T :  new()
{
	T loader = new();
	string[] lines = File.ReadAllText($"{Application.dataPath}/@CsvData/{csvFileName}Data.csv").Split("\n");
	for (int y = 1; y < lines.Length; y++)
	{
		string[] row = lines[y].Replace("\r", "").Split(',');
		if (row.Length == 0 || string.IsNullOrEmpty(row[0])) continue;
		MappingData(csvFileName, row, loader);
	}
	string jsonFile = JsonConvert.SerializeObject(loader, Formatting.Indented);
	File.WriteAllText($"{Application.dataPath}/@JsonData/{csvFileName}Data.json", jsonFile);
	AssetDatabase.Refresh();
}
#endregion
```

- 메서드는 CSV 파일을 파싱하고 그 결과를 JSON 파일로 저장하는 기능을 수행합니다. 제네릭 타입 `T`를 이용하여 이 메서드를 다양한 타입에 대해 재사용할 수 있게 해줍니다.

- `ParseData<T>`: 이것은 제네릭 메서드입니다. `<T>`는 메서드가 어떤 타입에도 동작할 수 있음을 나타냅니다. 호출하는 시점에 구체적인 타입을 지정하여 메서드를 인스턴스화합니다.
    
- `(string csvFileName)`: 메서드의 매개변수로, 파싱할 CSV 파일의 이름을 문자열로 받습니다.
    
- `where T : new()`: 이는 제약 조건(constraint)입니다. `new()` 제약 조건은 `T`가 반드시 기본 생성자를 가지고 있어야 함을 의미합니다. 즉, `new T()`와 같은 방식으로 인스턴스를 생성할 수 있는 타입만 `T`로 사용될 수 있습니다.


#### Mapping

```csharp
#region Mapping Method
/// <summary>
///  DataMapping Method
/// </summary>
/// <param name="csvFilename"></param>
/// <param name="row"></param>
/// <param name="loader"></param>
/// <typeparam name="T"></typeparam>
private static void  MappingData<T>(string csvFilename,string[] row, T loader) where T : new()
{
	switch (csvFilename)
	{
		case "Enemy":
			EnemyData(row, loader as EnemyDataLoader);
			break;
		case "Skill":
			SkillData(row, loader as SkillDataLoader);
			break; 
		case "Stage":
			StageData(row,loader as StageDataLoader);
			break;
	}
}
#endregion
```

- 파일 이름을 조건으로 검색하여 해당 메서드만을 호출합니다.

- 이때 호출 당시의 Loader Type을 매개변수로 전달 받아 해당 Loder를 참조합니다.

#### Casting

- 프로그래밍, 특히 C# 및 기타 강력한 형식의 언어에서 캐스팅은 한 데이터 형식의 값을 다른 데이터 형식으로 변환하는 작업입니다. 

- 변수를 캐스팅해야 하는 데에는 여러 가지 이유가 있지만 한 유형의 인스턴스를 다른 유형인 것처럼 처리하려는 경우, 유형 계층 내에서 또는 기본 유형 간에 변환할 때 자주 사용됩니다.

```csharp
EnemyData(row, loader as EnemyDataLoader);
```

- C#의 `as` 키워드는 개체를 유형으로 캐스팅하는 데 사용되며, 캐스팅을 수행할 수 없는 경우 예외를 발생시키는 대신 `null`을 반환합니다. 

- 이는 캐스트가 성공할지 확실하지 않고 'InvalidCastException'을 피하고 싶을 때 유용

- 이 코드에서 `loader as EnemyDataLoader`는 `loader`를 `EnemyDataLoader` 유형으로 캐스팅하려고 시도합니다. 

- `loader`가 실제로 `EnemyDataLoader`의 인스턴스이거나 파생 클래스인 경우 캐스트가 성공하고 `EnemyDataLoader` 유형의 `loader`에 대한 참조를 얻게 됩니다. 

- `loader`가 올바른 유형이 아닌 경우 결과는 `null`이 됩니다.

마치며
-- 
Generic 을 활용하면, 여러 타입에 대한 대응이 원활해져 코드의 양이 줄고 재사용성이 향상되 어떤 상황에 있어 대응이 원활해지는 장점이 있습니다.

특히 객체지향에서 클래스는 다르지만, 비슷한 역할 및 연산을 하는 경우 특히 상속 관계에 있는 인스턴스의 겨우 코드의 사용이 간결해지는 정말 좋은 기능입니다.

비슷한 구문의 코드라면 꼭 Generic을 활용한 프로그래밍을 경험해보시기 바랍니다.
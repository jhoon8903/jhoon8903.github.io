---
layout: post
title: Csv Parse To Json
subtitle: CSV를 Unity Editor에서 Json Parsing 하기
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
 - Editor
banner:
  image: https://www.imediacto.com/wp-content/uploads/2021/02/xml-csv-json-data-formats.png
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Data Parsing 자동화
--
### Editor Customize

- Unity에서는 `EdirotWindow`를 상속받으면 Unity Editor에 접근할 수 있는 스크립트에 대한 작성이 가능해집니다.

- 만약  상단 Menu에 Tool 탭을 만들고 하위 에 ParseCSV 라는 메뉴를 만들고자 한다면 다음과 같이 가능합니다.

```csharp
public class DataParser : EditorWindow  
{
#if UNITY_EDITOR
	[MenuItem("Tool/ParseCSV")]
	public static void ParseCsv()
	{
		ParseEnemyData("Enemy");
	}
#endif
}
```

- `MenuItem`이라는 Attribute를 생성하여 Editor에 `Tool`이라는 메뉴를 만듭니다.
- 이제 Editor를 새로고침이 되면 해당 Tool 을 확인이 가능해집니다.

![](https://i.imgur.com/XpX4vUk.jpg)

![](https://i.imgur.com/isqMoVS.jpg)

- 이제 프로젝트 시작 전 `ParseCSV`를 선택하면 자동적으로 csv 파일을 Json으로 파싱하여 데이터를 생성합니다.

### Parsing Csv

#### EnemyData.cs

- csv 파일 양식이 맞는 Data Type의 변수들을 선언하여 클래스를 생성
- Dictionary<`string`, `TObject`>를 Pair로 하는 ILoadData 인터페이스를 상속받는 전용 `EnemydataLoader` 클래스를 직렬화가능하도록 생성합니다.
- 이제 해당 Loader를 Access하면 `EnemyData`타입의 리스트를 생성할 수 있습니다.

```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
  
[Serializable]  
public class EnemyData  
{  
	/// <summary>  
	/// 적 Data를 사용하기 위한 Unique Key  
	/// </summary>  
	public enum EnemyKey  
	{  
		BOSS_MWJ, // 문원정님  
		SOLDIER1_MWJ,  
		SOLDIER2_MWJ,  
		SOLDIER3_MWJ,  
		  
		BOSS_CHH, // 최현호님  
		SOLDIER1_CHH,  
		SOLDIER2_CHH,  
		SOLDIER3_CHH,  
		  
		BOSS_LJS, // 이정훈님  
		SOLDIER1_LJH,  
		SOLDIER2_LJH,  
		SOLDIER3_LJH,  
		  
		BOSS_JEH, // 전은하님  
		SOLDIER1_JEH,  
		SOLDIER2_JEH,  
		SOLDIER3_JEH,  
		  
		BOSS_KSJ, // 김세진님  
		SOLDIER1_KSJ,  
		SOLDIER2_KSJ,  
		SOLDIER3_KSJ,  
	}  
	  
	public enum FireType { BOSS, STRAIGHT, CIRCLE, SECTORGORM }  
	public EnemyKey enemyKey;  
	public float speed;  
	public int HP;  
	public int Damage;  
	public FireType fireType;  
}  
	  
[Serializable]  
public class EnemyDataLoader : ILoadData<string, EnemyData>  
{  
	public List<EnemyData> enemies = new List<EnemyData>();  
	public Dictionary<string, EnemyData> MakeData()  
	{  
		return enemies.ToDictionary(enemy => enemy.enemyKey.ToString());  
	}  
}
```


#### DataParser.cs

- `string[] lines` 는 해당 CSV 파일의 Text 정보를 입력 받고 `\n`줄바꿈으로 구분하여 배열을 만듭니다.
- 반복문의 경우 csv Data 내부의 `,`를 기준으로 Data를 분리하여 배열의 index로 접근 가능하도록 해줍니다.
- Read된 파일 리스트를 Json으로 Convert하여 Json Data를 생성합니다.

```csharp
public class DataParser : EditorWindow  
{
#if UNITY_EDITOR
	[MenuItem("Tool/ParseCSV")]
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
			  
			loader.enemies.Add(new()  
			{  
			enemyKey = (EnemyData.EnemyKey)Enum.Parse(typeof(EnemyData.EnemyKey),row[0]),  
			speed = float.Parse(row[1]),  
			HP = int.Parse(row[2]),  
			Damage = int.Parse(row[3]),  
			fireType = (EnemyData.FireType)Enum.Parse(typeof(EnemyData.FireType),row[4]),  
			});  
		}  
		  
		string jsonFile = JsonConvert.SerializeObject(loader, Formatting.Indented);  
		File.WriteAllText($"{Application.dataPath}/@JsonData/{csvFileName}Data.json", jsonFile);  
		AssetDatabase.Refresh();  
	}
#endif
}
```

마치며
--

해당 기능을 통해 일일이 Json 파일을 작성하지 않고 버튼하나로 자동화 된 Json 데이터를 얻을 수 있어 기능 개발에 및 수정사항에 대한 대응을 빨라 질 수 있습니다.
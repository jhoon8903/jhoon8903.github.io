---
layout: post
title: DataManager의 역할
subtitle: 프로젝트의 DataManager의 역할
author: Daniel
categories: Project
tags: 
 - Unity
 - Project
 - Manager
banner:
  image: https://play-lh.googleusercontent.com/RFU-XUOtkOHfzkJYpUs5yOFS0CO3dgNNeMMCdGT-7DUgezsWHFHv_bRgo9ElK9CYhQ
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

DataManager
--
- DataManager는 프로젝트에 사용되는 Resource 를 제외한 모든 데이터를 관리하는 Manger 그룹입니다.
- 해당 프로젝트에서는 `Enemy`, `Stage`, `Skill` 등 다양한 데이터 정보를 csv파일에 작성
- 해당 csv 파일을 Json 형태로 변환하여 다른 팀원이 해당 데이터를 사용 가능하도록 변형하는 역할도 겸합니다.
- 문제는 Data의 타입의 무엇이 추가적으로 생길 지 프로젝트를 진행하면서 알 수 있기 때문에 일반적인 확장 가능한 Script를 작성해야 한다는 것입니다.
- `Interface`와 <`T`> 을 사용하여 일반적으로 사용가능한 스크립트 구성을 해야 합니다.

아래는 초기 ProtoType의 DataManager입니다.

### Manager
#### DataManager.cs
- 데이터를 게임 시작시 Service 등록하고 다른 Script에서 해당 서비스에 접근 할 수 있도록 합니다.

```csharp
public class DataManager : MonoBehaviour  
{  
	public Dictionary<string, EnemyData> Enemies = new Dictionary<string, EnemyData>();  
	private AddressableAsset _addressableAsset;  
	  
	private void Awake()  
	{  
		ServiceLocator.RegisterService(this);  
	}  
	  
	private void Start()  
	{  
		_addressableAsset = ServiceLocator.GetService<AddressableAsset>();  
	}  
	  
	public void InitializeData()  
	{  
		Enemies = LoadJson<EnemyDataLoader, string, EnemyData>("EnemyData").MakeData();  
	}  
	  
	private TLoader LoadJson<TLoader, TKey, TValue>(string path) where TLoader : ILoadData<TKey, TValue>  
	{  
		TextAsset textAsset = _addressableAsset.Load<TextAsset>(path);  
		Debug.Log(textAsset.text);  
		return JsonConvert.DeserializeObject<TLoader>(textAsset.text);  
	}  
}
```

### Utility
- Data의 Convert 또는 Load 를 위한 Utility 스크립트를 작성합니다.
- 본 프로젝트는 Addressable Asset을 사용할 예정이며, 이는 ResourceManager를 이용하여 작성할 예정입니다.
#### ServiceLocator.cs
- ServiceLocator 패턴을 이용하기 위한 static class 작성

```csharp
using System;  
using System.Collections.Generic;  
  
public static class ServiceLocator  
{  
	// 서비스들을 저장하는 Dictionary. Type을 키로 하고, 서비스 객체를 값으로 함.  
	private static readonly Dictionary<Type, object> Services = new Dictionary<Type, object>();  
	  
	/// <summary>  
	/// 서비스를 등록하는 메서드.  
	/// 제네릭 타입 T를 사용하여 어떤 타입의 서비스든 등록할 수 있음.  
	/// </summary>  
	/// <param name="service">등록할 서비스 객체</param>  
	public static void RegisterService<T>(T service)  
	{  
		// 서비스 객체를 Dictionary에 추가. 이미 존재하는 경우 해당 타입의 서비스를 덮어씀.  
		Services[typeof(T)] = service;  
	}  
	  
	/// <summary>  
	/// 등록된 서비스를 검색하는 메서드.  
	/// 제네릭 타입 T를 사용하여 원하는 타입의 서비스를 검색할 수 있음.  
	/// </summary>  
	/// <returns>검색된 서비스 객체. 해당 타입의 서비스가 없으면 예외 발생.</returns>  
	public static T GetService<T>()  
	{  
		// 요청된 타입의 서비스를 Dictionary에서 찾아 반환.  
		// 해당 타입의 서비스가 없는 경우 예외가 발생할 수 있음.  
		return (T)Services[typeof(T)];  
	}  
}
```

#### AddressableAsset.cs
- Test용 Addressable class 작성

```csharp
using System;  
using System.Collections.Generic;  
using UnityEngine;  
using UnityEngine.AddressableAssets;  
using UnityEngine.ResourceManagement.AsyncOperations;  
using Object = UnityEngine.Object;  
  
public class AddressableAsset : MonoBehaviour  
{  
	private Dictionary<string, Object> resources = new();  
	private DataManager _dataManager;  
	  
	private void Awake()  
	{  
		ServiceLocator.RegisterService(this);  
	}  
	  
	private void Start()  
	{  
		_dataManager = ServiceLocator.GetService<DataManager>();  
		LoadAllAsync<Object>("PreLoad", (s, i, arg3) =>  
		{  
			_dataManager.InitializeData();  
		});  
	}  
	  
	public void LoadAllAsync<T>(string label, Action<string, int, int> callback) where T : Object  
	{  
		var operation = Addressables.LoadResourceLocationsAsync(label, typeof(T));  
		operation.Completed += op =>  
		{  
		int loadCount = 0;  
		int totalCount = op.Result.Count;  
		  
			foreach (var result in op.Result)  
			{  
				LoadAsync<T>(result.PrimaryKey, obj =>  
				{  
					loadCount++;  
					callback?.Invoke(result.PrimaryKey, loadCount, totalCount);  
				});  
			}  
		};  
	}  
	  
	private void LoadAsync<T>(string key, Action<T> cb = null) where T : Object  
	{  
		if (resources.TryGetValue(key, out Object resource))  
		{  
			cb?.Invoke(resource as T);  
			return;  
		}  
	  
		string loadKey = key;  
		  
		AsyncOperationHandle<T> asyncResource = Addressables.LoadAssetAsync<T>(loadKey);  
		asyncResource.Completed += asyncOperationHandle =>  
		{  
			resources.Add(key, asyncOperationHandle.Result);  
			cb?.Invoke(asyncOperationHandle.Result);  
		};  
	}  
	  
	public T Load<T>(string key) where T : Object  
	{  
		if (!resources.TryGetValue(key, out Object resource)) return null;  
		return resource as T;  
	}  
}
```


마치며
--
처음 해보는 패턴의 작업이라 기존 참고 스크립트를 이해하는데 시간이 꽤 걸리고 있습니다.
다만, 이해를 하고 응용할 줄 안다면 빠르고 확장성이 보장된 서비스를 구축할 수 있을 것으로 예상되어 꼭 해결해야 할 것 입니다.
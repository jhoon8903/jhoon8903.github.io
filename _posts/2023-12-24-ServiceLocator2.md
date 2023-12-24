---
layout: post
title: Service Locator 개선
subtitle: 동적 인스턴스 생성으로 Sevice Locator 이용하기
author: Daniel
categories: Unity
tags: 
 - Programming
 - Unity
 - Project
banner:
  image: https://i.imgur.com/kaCR1JP.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

기존 ServiceLocator의 경우 서비스를 등록하고 사용해야하는 방법으로 사용하고 있었는데 
이번 프로젝트를 진행하면서, GatOrAddComponent 처럼 동적으로 사용할 수 있을 것 같아
코드를 변경한 리팩토링을 진행 합니다.

### 기존 ServiceLocator

```csharp
using System;  
using System.Collections.Generic;  
  
public static class ServiceLocator  
{     
	private static readonly Dictionary<Type, object> Services = new();     
    {     
        Services[typeof(T)] = service;    
	}    
	 
    public static T GetService<T>()    
    {          
	    return (T)Services[typeof(T)];    
	}  
}
```

- 일일이 서비스를 등록하고 사용을 해야 하지만, 서비스 등록을 하기 전에 서비스에 접근하는 경우에 대해서 확인하고 컨텍스트 순서에 맞추어 등록을 해주어야 하는 불편함이 있습니다.
- 이를 서비스 검색 시 등록이 되어 있다면 사용하고, 등록이 되어  있지 않다면 등록하여 사용하는 방법으로 리팩토링 합니다.

### 개선된 ServiceLocator

```csharp
using System;  
using System.Collections.Generic;  
  
public static class ServiceLocator  
{  
    private static readonly Dictionary<Type, object> Services = new();  
  
    public static T GetService<T>() where T : class  
    {  
        Type serviceType = typeof(T);  
        
        if (Services.TryGetValue(serviceType, out object service))  
        {            
	        return (T)service;  
        }        
        
        return TryCreateService<T>(serviceType);  
    }  
    private static T TryCreateService<T>(Type serviceType) where T : class  
    {  
        T serviceInstance = Activator.CreateInstance<T>();  
        Services[serviceType] = serviceInstance;  
        return serviceInstance;  
    }
}
```

- 개선된 코드에서는 `T serviceIntance = Activator.CreateIntance<T>()`지정된 타입 `T`의 새 인스턴스를 동적으로 생성하는데 사용됩니다.
- 프로그램 실행 중 타입이 결정되는 경우, 이 메서드를 사용하여 객체를 동적으로 생성하여 사용이 가능해 집니다.
- 이 방식은 리플렉션을 사용하기 때문에 직접 인스턴스를 생성하는 것 보다는 느릴 수 있지만, 일반적으로는 별로 성능의 차이가 크지 않습니다.
- ServiceLocator를 사용하면 `싱글턴을 사용하지 않고`각 Manager, Data, Resource에 접근이 가능해집니다.

### BaseScene.cs 사용 예

```csharp
using System;  
using Manager;  
using UnityEngine;  
using UnityEngine.EventSystems;  
using Object = UnityEngine.Object;  
  
public class BaseScene : MonoBehaviour  
{  
	public static Label CurrentScene { get; set; }  
	protected ResourceManager resourceManager;  
	protected UIManager uiManager;  
	protected ScenesManager scenesManager;  
	
	private void Awake()  
	{            
		resourceManager = ServiceLocator.GetService<ResourceManager>();  
		uiManager = ServiceLocator.GetService<UIManager>();  
		scenesManager = ServiceLocator.GetService<ScenesManager>();  
	}  
	
	private void Start()  
	{            
		if (resourceManager.Preload) Initialize();  
		else  
		{  
			resourceManager.AllLoadResource<Object>("Preload", (key, count, totalCount) =>  
			{  
				Debug.Log($"[Preload] Load asset {key} ({count}/{totalCount})");  
				if (count < totalCount) return;  
				resourceManager.Preload = true;  
				Initialize();  
			});            
		}        
	}  
	
	protected virtual void Initialize()  
	{            
		Object eventSystem = FindObjectOfType<EventSystem>();  
		if (eventSystem == null) resourceManager.InstantiateObject("EventSystem").name = "@EventSystem";  
	}    
}
```
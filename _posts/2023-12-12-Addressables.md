---
layout: post
title: Addressables Asset 기초
subtitle: 설치부터 기본 Load 까지
author: Daniel
categories: Unity
tags: 
 - Unity
 - Resource
 - Asset
banner:
  image: https://i.imgur.com/Kpt4GBr.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Addressables
--

- **Addressables** 에셋은 리소스를 효율적으로 관리하고자 Unity에서 기본적으로 제공하는 에셋입니다.
- Resources 폴더의 경우 사용 여부와는 상관없이 모두 Load를 하는데 적은양의 리소스의 경우 문제가 없지만 프로젝트의 사이즈가 커져 관리해야 하는 리소스가 많이 생기게 되면, 게임 로드시에 렉이나 문제를 유발 할 수 있습니다.
- Addressables 의 경우 이런 문제를 해결하고 `Label`을 이용하여 원하는 시점에 리소스들을 로드할 수 있습니다.
- 예를들어, Lobby Scene 리소스, Game Scene 리소스 등을 구분하여 로드가 가능해집니다.

설치
--
- `[Window] => [Package Manager] => Package : Unity Registry` => Addressables를 Install 합니다.

![](https://i.imgur.com/IG8GuIv.jpg)

리소스 등록 및 관리
--
- `[Window] => [Assets Management] => Addressables => Group`

![](https://i.imgur.com/6gfcEH6.jpg)

### Addressables Group

![](https://i.imgur.com/Kpt4GBr.jpg)

- **New** : 새 그룹을 생성합니다.
- **Group** : 종류별로 임의의 그룹에 따라서 리소스를 정리 할 수 있습니다.
- 등록 : Project 에서 Drag & Drop으로 등록합니다.
- 삭제 : 리소스를 선택 후 오른쪽 클릭하여 Remove 합니다.

![](https://i.imgur.com/iK9euTU.jpg)

- **PrimeKey** : `노란색` 박스의 안에 있는 것은 `PrimeKey`로 해당 리소스를 특정할 수 있는 Key값 입니다. 처음 등록시 Path가 모두 나오게 되므로 이름을 수정해주어야 편합니다.
- **Path** : `주황색` 박스안의 내용이며, 해당 리소스의 경로(Path)값을 나타냅니다.
- **Label** : `파란색` 박스안의 있는 것은 `Label`로 임의의 라벨을 생성할 수 있으며, 추가하거나 삭제 할  수 있습니다. 언제 Load할 것인지 구분할 때 사용할 수 있습니다.

>❗️ 주의 할 점은 Path에 최종적인 `File Name`과 `PrimeKey` 의 이름이 같아야 합니다. 이름이 다르면 리소스를 찾을 수 없다는 Error Log를 만나게 됩니다. (PrimeKey는 별칭(as)이 아닙니다.)

Code
--
### LoadAsync

- `PrimeKey`가 일치하는 리소스를 로드하는 메서드
- Callback Action으로 해당 리소스를 로드 합니다.

```csharp
public void LoadAsync<T>(string key, Action<T> cb = null) where T : Object  
{  
    string loadKey = key;  
    
    if (_resources.TryGetValue(key, out Object resource))  
    {        
	    cb?.Invoke(resource as T);  
        return;  
    }    
    
    if (key.Contains(".sprite"))  
    {        
	    loadKey = $"{key}[{key.Replace(".sprite", "")}]";  
        AsyncOperationHandle<Sprite> handle = Addressables.LoadAssetAsync<Sprite>(loadKey);  
        HandleCallback(key, handle, cb as Action<Sprite>);  
    }    
    else  
    {  
        AsyncOperationHandle<T> handle = Addressables.LoadAssetAsync<T>(loadKey);  
        HandleCallback(key, handle, cb);  
    }
}
```

#### Addressables.LoadAssetAsync<`T`>(loadKey)

- 어드레서블 그룹중에서 Key를 입력 받아 해당 타입의 오브젝트를 반환합니다.
- 만약 해당 키가 존재하지 않는다면 Exception을 출력합니다.

### LoadAllAsync

- `label`을 확인하여 해당 라벨에 포함되는 모든 리소스를 로드합니다.

```csharp
public void LoadAllAsync<T>(string label, Action<string, int, int> cb)  where T : Object  
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
                cb?.Invoke(result.PrimaryKey, loadCount, totalCount);  
            });        
		}    
	};    
	Loaded = true;  
}
```

#### Callback Action

- Addressable.Load.... 메서드의 경우 비동기적으로 움직이기 때문에 리소스가 로드되고 있는 와중에도 다른 프롤세스를 처리할 수 있습니다.
- 그렇게 때문에 해당 로드가 완료된 이벤트를 실행시켜야 정상적으로 다음 코드를 실행할 수 있습니다.
- 비동기 로드가 끝나고 호출되는 이벤트는 `.Complete`가 호출 됩니다.
- 해당 이벤트가 호출 되면 Callback을 실행할 수 있도록 코드를 작성합니다.
- 해당 코드에서는 PrimeKey, 로드된 리소스의 개수, 총 로드를 해야하는 리소스 개수를 Invoke 합니다.

- 해당 메서드의 실제 사례는 아래와 같습니다.

```csharp
MainManager.ResourceManager.LoadAllAsync<Object>("PreLoad", (key, count, totalCount) =>  
{  
    Debug.Log($"{key} Load ({count} / {totalCount})");  
    
    if (count < totalCount) return;  
    
    MainManager.ResourceManager.Loaded = true;  
    
    Initialize();  
});
```

- Debug.Log 결괏값은 아래와 같습니다.

![](https://i.imgur.com/rRz5eem.jpg)

- 해당  Callback을 이용하면 Loading Bar등을 구현할 수 있습니다.
- Slider의 value = count, MaxValue = totalCount를 할당하면 리소스가 로드 될때마다 슬라이더가 채워집니다.


아래는 Addressables 기능의 전체 코드 입니다.

```csharp
private void HandleCallback<T>(string key, AsyncOperationHandle<T> handle, Action<T> cb) where T : Object  
{  
    handle.Completed += operationHandle =>  
    {  
        _resources.Add(key, operationHandle.Result);  
        cb?.Invoke(operationHandle.Result);  
    };
}  
  
public void LoadAsync<T>(string key, Action<T> cb = null) where T : Object  
{  
    string loadKey = key;  
    if (_resources.TryGetValue(key, out Object resource))  
    {        
	    cb?.Invoke(resource as T);  
        return;  
    }    
    if (key.Contains(".sprite"))  
    {  
		loadKey = $"{key}[{key.Replace(".sprite", "")}]";  
        AsyncOperationHandle<Sprite> handle = Addressables.LoadAssetAsync<Sprite>(loadKey);  
        HandleCallback(key, handle, cb as Action<Sprite>);  
    }    
    else  
    {  
        AsyncOperationHandle<T> handle = Addressables.LoadAssetAsync<T>(loadKey);  
        HandleCallback(key, handle, cb);  
    }
}  
  
public void LoadAllAsync<T>(string label, Action<string, int, int> cb)  where T : Object  
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
                cb?.Invoke(result.PrimaryKey, loadCount, totalCount);  
            });        
		}    
	};    
	Loaded = true;  
}
```
마치며
--
어드레서블의 경우 처음에는 익숙하지 않아서 시간이 좀 걸리지만, 이해하고 나면 정말 편한 기능을 제공합니다.

다만 위에서 알아본 기능은 정말 많은 기능 중 하나에 불과하므로 기회가 된다면 다른 기능도 이용해 보았으면 합니다.
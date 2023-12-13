---
layout: post
title: 어드레서블 멀티 스프라이트 
subtitle: 멀티보드의 스프라이트는 어떻게 가져올까?
author: Daniel
categories: Unity
tags: 
 - Unity
 - Addressables
banner:
  image: https://i.imgur.com/FoCVIh5.jpg
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

문제 발생
--

Addressables 를 사용하다 얘기치 못한 문제에 마딱드림..

![](https://i.imgur.com/FidPray.jpg)

기존의 사용방법으로는 멀티모드의 Sprite 형태의 리소스를 가지고 올 수 없는 문제가 있습니다.
지금 코드의 Addressable은 PrimeKey 값을 `Armor01[Armor01]` 이렇게 가져오고 있는데
멀티에서는 보는 것과 같이 Body, Left, Right 등 의도하지 않은 문자가 들어서 데이터를 로드할 수 없는 문제가 발생합니다.

별도로 하위 녀석에게 접근하여 바꿀 수 없는 문제가 있어 해결에 난항을 겪고 있는 와중 
2016년 자료를 보게 되었는데 이 당시에는 문제가 많았던 기능이었지만 지금은 가능하지 않을까해서 Source Code를 다 뒤져보았습니다.

![](https://i.imgur.com/FoCVIh5.jpg)

![](https://i.imgur.com/yNSSY08.jpg)

- 해당 소스코드를 보면 기존 Load 스크립트와 다르다는 것을 알수 있다.
단일 객체가 아닌 다중 객체를 가져오기 위해 IList<> 를 사용하여 멀티 Sprite를 처리할 수 있습니다.

수정된 코드는 다음과 같습니다.

```csharp
private void HandleCallback<T>(string key, AsyncOperationHandle<T> handle, Action<T> cb) where T : Object  
{  
    handle.Completed += operationHandle =>  
    {  
        _resources.Add(key, operationHandle.Result);  
        cb?.Invoke(operationHandle.Result);  
    };}  
  
private void HandleCallback<T>(string key, AsyncOperationHandle<IList<T>> handle, Action<IList<T>> cb) where T : Object  
{  
    handle.Completed += operationHandle =>  
    {  
        IList<T> resultList = operationHandle.Result;  
        // 리스트의 각 아이템을 _resources에 추가합니다.  
        
        for (int i = 0; i < resultList.Count; i++)  
        {            
	        string resourceKey = $"{key}[{i}]"; // 리스트 아이템에 대한 고유 키  
            _resources.Add(resourceKey, resultList[i]);  
        }        
        cb?.Invoke(resultList);  
    };
}  
  
private void LoadAsync<T>(string key, Action<T> cb = null) where T : Object  
{  
    string loadKey = key;  
    
    if (_resources.TryGetValue(key, out Object resource))  
    {        
	    cb?.Invoke(resource as T);  
        return;  
    }  
    
    if (key.Contains(".multiSprite"))  
    {            
	    AsyncOperationHandle<IList<Sprite>>handle = Addressables.LoadAssetAsync<IList<Sprite>>(loadKey);  
            HandleCallback<Sprite>(key, handle, objs => cb?.Invoke(objs as T));  
    }    
    else if (key.Contains(".sprite"))  
    {        
	    // 싱글 스프라이트 처리  
        AsyncOperationHandle<Sprite> handle = Addressables.LoadAssetAsync<Sprite>(loadKey);  
        HandleCallback(key, handle, cb as Action<Sprite>);  
    }    
    else  
    {  
        // 일반 에셋 처리  
        AsyncOperationHandle<T> handle = Addressables.LoadAssetAsync<T>(loadKey);  
        HandleCallback(key, handle, cb);  
    }}
```

- 로드하고자 하는 객체들을 IList로 묶어 로드 할 수 있으면 calback 또한 묶어서 처리할 수 있게 됩니다.

- 해당 방법으로 리소스 로드 자체는 성공했지만 아직 사용할 수 있는 상태는 아니게 된다 해당 리소스를 어떤 키로 부를 수 있는지 좀 더 연구 해봐야 한다.
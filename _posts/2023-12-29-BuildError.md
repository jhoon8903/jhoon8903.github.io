---
layout: post
title: 게임 빌드 트러블
subtitle: 왜 GameScene Resource는 로드가 되지 않는가?
author: Daniel
categories: Unity
tags: 
 - Unity
 - Programming
banner:
  image:
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

문제점
--
- 빌드 상태와 에디터의 상태가 달라 빌드시 게임 리소스가 정상적으로 로드 되지 않는 문제 발생

| 에디터 | 빌드 |
| :--: | :--: |
| ![](https://i.imgur.com/pXSQS79.gif)<br> | ![](https://i.imgur.com/it6TrdQ.gif) |

### 문제 확인 1. Console Asset을 이용한 런타임 에러 확인

- `Quantum Console` 에셋을 이용한 빌드 런타임 디버깅

| 에디터 | 빌드 |
| :--: | :--- |
| ![](https://i.imgur.com/wS1g15G.gif) | ![](https://i.imgur.com/88xVPZd.gif) |
- 에디터 상에서는 GameScene Load가 정상적으로 작동

![](https://i.imgur.com/YlP9FYJ.jpg)

- 빌드 상에서는 GameScene Load가 작동하지 않는 문제 확인

![](https://i.imgur.com/oLauV9Q.jpg)

### 컨텍스트 Debug.Log() Flag

- 전체적인 컨텍스트에 대한 디버그 로그를 작성하여 빌드상에서 문제를 확인
- 비동기 메서드 인 `AllLoadResource` 메서드 확인

```csharp
public void AllLoadResource<T>(string label, Action<string, int, int> callback) where T : Object  
{  
    Debug.Log($"리소스매니져 37");  
    
    AsyncOperationHandle<IList<IResourceLocation>> operation =
    Addressables.LoadResourceLocationsAsync(label, typeof(T));  
    
    Debug.Log($"리소스매니져 39 - Operation : {operation}");  
    
    operation.Completed += operationHandle =>  
    {  
        Debug.Log($"리소스매니져 42 - Operation Call Back");  
    
		int loadCount = 0;  
        int totalCount = operationHandle.Result.Count;  
        foreach (var result in operationHandle.Result)  
        {            
	        Debug.Log($"리소스매니져 47 - Operation Call Back - Foreach");  
            LoadResource<T>(result.PrimaryKey, obj =>  
            {  
                Debug.Log($"리소스매니져 450 - Operation Call Back - Foreach - LoadResource Method Callback");  
                
                loadCount++;
                callback?.Invoke(result.PrimaryKey, loadCount, totalCount);  
            });        
		}    
	};
}  
  
private void LoadResource<T>(string key, Action<T> callback = null) where T : Object  
{  
    Debug.Log($"리소스 매니져 60  - LoadResource Method");  
    
    IResourceLoader loader = GetLoader(key);  
    
    loader?.LoadResource<T>(key, obj =>  
    {  
		Debug.Log($"리소스 매니져 64  - LoadResource Method - Loader Interface Callback");  
        
        Resource[key] = obj;  
        callback?.Invoke(obj);  
    });
}
```

- 별도의 특이사항을 체크하지 못했으며, 의도한대로 제대로 실행되고 있는 것을 확인

### 특이사항 발견

- 전체 컨텍스트 확인 및 지속적인 로그 확인 중 특이사항 발견

**빌드 버전**

![](https://i.imgur.com/ajksl3a.png)

**에디터 버전**

![](https://i.imgur.com/AHxt9C7.png)

- 에디터에서는 15개의 리소스를 로드
- 빌드 버전에서는 16개의 리소스를 로드 하는 문제 확인

### Addressable 리소스 그룹 확인

![](https://i.imgur.com/r5qHLpm.jpg)

- 실제 "Preload" 라벨을 가지고 있는 에셋의 숫자는 15개 
- 에디터의 리소스 로드의 숫자가 맞는것을 확인
- 빌드시 에디터의 상태가 아닌 다른 상태가 적용되는 것을 확인

### Addressable Cache 초기화

![](https://i.imgur.com/7JHrt5i.jpg)

- Addressable => Clear Build Cache => All
- 캐시를 전체 지우고 다시 빌드를 진행

![](https://i.imgur.com/6nydUsF.jpg)

- 아직도 16개의 리소스가 로드 되는 것을 확인

### 중복되는 Resource 확인

- 같은 리소스가 2회 로드 됨

![](https://i.imgur.com/jGylDBt.jpg)

- Addressable 을 확인 하였지만 2번 실행될 이유가 없음
- 해당 리소스를 삭제 후 빌드 진행하여 해당 리소스가 로드 되는지 확인 필요

![](https://i.imgur.com/pY3oYP4.jpg)

- 14개가 로드 됨
- 다시 추가하여 확인

![](https://i.imgur.com/W1c7iCL.jpg)

- 다시 중복된 로드로 16개의 리소스가 로드 되는 것을 확인
- 에디터 상에서 다시 확인

![](https://i.imgur.com/GN2cBeA.jpg)

- 한번만 로드 되는 것 확인

### Replace가 아닌 완전 새로운 빌드

- 기존에는 빌드시 같은 Path에 있는 파일을 Replace 하여 빌드 생성
- 앱을 완전 삭제 후 다시 빌드 진행
- 결과 바뀌지 않음

### Development Build && Script Debugging

- 빌드 시 사용가능한 디버깅 기능 사용
  
![](https://i.imgur.com/zzW9wAt.jpg)

- 빌드를 실행하면 다음과 같은 팝업이 확인 됩니다.

![](https://i.imgur.com/yrIBWNA.jpg)

- 해당 포트로 IDE 디버거를 연결 합니다.
- Rider의 경우 디버거 상단의 Attach to Unity Process 를 실행합니다.

![](https://i.imgur.com/2MnAaIV.jpg)

- 디버깅 리스트 중 해당 포트를 확인하여 동일한 포트에 접근합니다.

![](https://i.imgur.com/OseMyK1.jpg)

- 이렇게 하면 빌드에 대한 Script Debugging이 가능해 집니다.


![](https://i.imgur.com/Vq2AUHe.jpg)


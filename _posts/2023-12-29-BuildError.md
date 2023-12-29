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
  image: https://i.imgur.com/pXSQS79.gif
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

### 기존 작성 스크립트 재사용

- 코드를 리팩토링 하기 전 정상 작동 하였던 지난 프로젝트 코드를 가져와 실행
	=> 동일증상 발생
	=> Resource Manager 스크립트 문제가 아닌 것으로 판단

### Addressable Build Setting 변경

- Addressable Asset의 빌드 타입을 변경

![](https://i.imgur.com/GrKOqBQ.jpg)

#### Use Asset Database (fastest)

- **목적** 
	- 가장 빠른 기본 옵션입니다. 
	- 일반적인 개발 및 반복에 이상적입니다.

- **동작** 
	- 에셋 데이터베이스에서 직접 로드됩니다. 
	- 이는 Addressable Asset System의 일반적인 로딩 프로세스(asset packing and catalog loading)를 우회합니다.

- **장점** 
	- 에셋 번들 시뮬레이션에 따른 오버헤드를 방지하므로 Unity 에디터에서 테스트하는 동안 로드 시간이 크게 단축됩니다.

- **사용 사례**
	- 에셋 번들 다운로드를 테스트하거나 에셋 번들 동작을 시뮬레이션할 필요가 없을 때 신속한 반복 및 테스트를 위해 이 모드를 사용합니다.

#### Simulate Groups (advanced)

- **목적**
	- 이 옵션은 고급 옵션이며 실제로 에셋 번들을 생성하지 않고 번들 로딩을 시뮬레이션합니다.

- **동작**
	- 에셋은 여전히 ​​에셋 데이터베이스에서 로드되지만 Unity는 마치 에셋 번들에서 로드된 것처럼 프로세스를 시뮬레이션합니다. 
	- 여기에는 카탈로그 및 자산 번들 로딩 시뮬레이션이 포함됩니다.

- **장점**
	- 실제 번들을 생성하는 오버헤드 없이 종속성 및 메모리 사용량을 포함하여 번들에서 에셋이 로드되는 방법을 테스트할 수 있습니다.

- **사용 사례**
	- 번들 구성, 종속성 및 메모리 사용량을 보다 현실적인 방식으로 테스트하는 동시에 시간이 많이 소요되는 번들 구축 프로세스를 방지하는 데 이상적입니다.

#### Use Existing Build (OSX)
- **목적**
	- 이 옵션을 사용하면 이전에 구축한 실제 에셋 번들로 테스트할 수 있습니다.

- **동작**
	- 에셋 데이터베이스가 아닌 이전에 구축된 에셋 번들에서 로드됩니다. 
	- 이를 위해서는 에셋 번들이 이미 구축되어 있어야 합니다.

- **장점**
	- 실제 에셋 번들을 사용하므로 런타임 동작에 대한 가장 정확한 시뮬레이션을 제공합니다.

- **사용 사례**
	- 이 모드를 사용하여 다운로드 시간, 번들 로딩 성능, 에셋 번들의 실제 런타임 동작을 포함한 최종 빌드 구성을 테스트합니다.

=> 결과적으로 어플리케이션을 실제 빌드 상태에서 어드레서블을 적용하려면 `3번째 옵션인 "Use Existing Build"`로 세팅을 바꾸고 진행 해야 합니다.

### Use Existing Build (OSX) 적용

![](https://i.imgur.com/JbYdKv4.jpg)

- 에디터 상에서도 빌드 상태와 동인할 상태가 되었고 이제 부터 다시 디버깅을 합니다.

### 리소스 로드 및 씬 로드 스크립트

```csharp
protected override void Initialize()  
{  
    base.Initialize();  
    CurrentScene = Label.IntroScene;  
    InstantiatePlayer();  
    InstantiateIntroUI();  
    LoadResource();  
    LoadAsyncOperation();  
    BackGround.InstantiateBackGround(Background, Scenes.BaseObjects.transform);  
}

private void LoadResource()  
{   
Resource.AllLoadResource<Object>($"{LabelString}", (key, count, totalCount) =>  
    {  
        Debug.Log($"[{LabelString}] Load asset {key} ({count}/{totalCount})");  
        _introUI.UpdateToProgress(count, totalCount, key);  
        if (count < totalCount) return;  
        Resource.GameSceneLoad = true;  
    });
}  
  
private void LoadAsyncOperation()  
{  
    AsyncOperation operation = LoadAsync(LabelString);  
    StartCoroutine(GameSceneLoad(operation));  
}  
  
private AsyncOperation LoadAsync(string scene)  
{   
	AsyncOperation operation = SceneManager.LoadSceneAsync(scene);  
    operation.allowSceneActivation = false;  
    return operation;  
}  
  
private IEnumerator GameSceneLoad(AsyncOperation operation)  
{   
	while (!Resource.GameSceneLoad && operation.progress < 0.9f) yield return null;  
    while (!Input.GetMouseButtonDown(0)) yield return null;  
    operation.allowSceneActivation = true;  
}
```

- Intro Scene에서 초기화 및 데이터 로드 그리고 비동기 씬 로드를 하는 코드입니다.
- 의도한 동작은 데이터는 비동기 적으로 로드가 되고, 씬도 비동기 적으로 로드가 되는데 
- `LoadSceneAsync` 에서 씬이 로드 되는동안 리소스가 같이 로드 되서 Progress 를 만족할 때 대기 하였다가 다음 씬으로 진입을 하는 것으로 예상 
- 하지만 실제로는 정확히 원인파악은 되지 않지만, 씬이 먼저 준비가 되어버린 상황에서 해당 리소스 로직이 중단 되어버리는 문제가 발생하여, 프로세스가 다음 씬으로 넘어가는 문제로 판단
- 리소스 로드 완료 콜백에 씬 로드 함수를 포함하여 리소스 로드 완료 후 씬을 준비 할 수 있도록 하였습니다.

```csharp
private void LoadResource()  
{   
	Resource.AllLoadResource<Object>($"{LabelString}", (key, count, totalCount) =>  
    {  
        Debug.Log($"[{LabelString}] Load asset {key} ({count}/{totalCount})");  
        _introUI.UpdateToProgress(count, totalCount, key);  
        if (count < totalCount) return;  
        Resource.GameSceneLoad = true;  
        LoadAsyncOperation();  
    });
}
```


마치며
--

해당 문제를 해결하는 약 15시간이 걸렸습니다.
특히 아무런 문제가 없는 코드에서 문제를 찾으려 하고 어드레서블의 사용 방법에 문제가 있다는 것을 알지 못하여 발생한 문제였습니다.

그리고 비동기 방식에 대한 이해도가 낮은것 같아 해당 컨텍스트의 원인을 찾지 못하는 것이 아쉽습니다.
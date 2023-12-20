---
layout: post
title: Loading Scene 생성
subtitle: Loading Scene 만들기
author: Daniel
categories: Unity
tags: 
 - Unity
 - Programming
 - Project
banner:
  image: https://i.imgur.com/aOtROKQ.gif
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

Loading Scene
--

![](https://i.imgur.com/aOtROKQ.gif)

- 각 씬을 넘어갈 때 마다, 리소스의 로드를 효과적으로 하기 위해 Loading Scene 에서 리소스를 로드하고, 비동기적으로 다음 씬을 로드 하는 방법으로 제작
### Loading Scene.cs

```csharp
using System.Collections;  
using Scripts.UI.Scene_UI;  
using UnityEngine;  
using UnityEngine.SceneManagement;  
using Object = UnityEngine.Object;  

public class LoadingScene : BaseScene  
{  

	private Loading_UI _loadingUI;  
	private int _count;  
	private int _totalCount;  
	private string _key;  
	private bool _resourcesLoaded = false;   
	
	protected override bool Initialized()  
	{            
		if (!base.Initialized()) return false;  
		Main.NextScene ??= "IntroScene";  
		LoadResourcesAndScene();  
		return true;  
	}  
	
	private void LoadResourcesAndScene()  
	{            
		LoadResources();  
	}  
	
	private void LoadResources()  
	{            
		_loadingUI = Main.UI.SetSceneUI<Loading_UI>();  
		Main.Resource.AllLoadAsync<Object>($"{Main.NextScene}", (key, count, totalCount) =>  
		{  
			_key = key;  
			_count = count;  
			_totalCount = totalCount;  
			_loadingUI.SetTotalCount(_totalCount);  
			_loadingUI.UpdateProgress(_count, _key);  
			Debug.Log($"[{Main.NextScene}] Load asset {_key} ({_count}/{_totalCount})");  
			if (_count != _totalCount) return;  
			_resourcesLoaded = true; // 리소스 로딩 완료  
			LoadNextSceneAsync(); // 리소스 로딩 완료 후 씬 로딩 시작  
		});  
	}  
	
	private void LoadNextSceneAsync()  
	{            
		AsyncOperation sceneLoadOperation = SceneManager.LoadSceneAsync(Main.NextScene);  
		sceneLoadOperation.allowSceneActivation = false;  
		StartCoroutine(UpdateLoadingProgress(sceneLoadOperation));  
	}  
	
	private IEnumerator UpdateLoadingProgress(AsyncOperation sceneLoadOperation)  
	{            
		while (!_resourcesLoaded || sceneLoadOperation.progress < 0.9f) yield return null;  
		_loadingUI.SetAnyPress();  
		while (!Input.anyKeyDown) yield return null;  
		sceneLoadOperation.allowSceneActivation = true;  
	}    
}
```

#### LoadSceneAsync

- Loading Scene에서 볼 것은 Scene을 비동기적으로 로드하는 기능입니다.

```csharp
AsyncOperation operation = SceneManager.LoadSceneAsync("씬이름"); 
```

- `LoadSceneAsync` 메서드의 경우 SceneManager에서 제공하는 메서드로 
- 이 방법은 현재 장면을 계속 실행하면서 백그라운드에서 장면을 로드하는 게임 개발에서 특히 유용합니다.

**Return**
- `AsyncOperation` 객체를 반환
- Scene Load 진행 상황을 확인하고 로드가 완료되면 작업을 수행하는 데 사용

#### 특징 및 특징

1. **비동기 로딩**:
    - 장면 로딩은 백그라운드에서 발생하므로 현재 장면이 계속 실행될 수 있습니다(종종 로딩 화면이나 애니메이션을 표시하는 데 사용됨).
    - 게임이 멈추거나 응답하지 않는 현상을 방지합니다. 이는 동기식 `SceneManager.LoadScene` 메서드에서 흔히 발생합니다.

2. **진행 상황 추적**:
    
    - `AsyncOperation`은 로딩 진행률을 나타내는 0과 1 사이의 부동 소수점 값인 `progress` 속성을 제공합니다.
    - 진행률 표시줄과 같은 로딩 UI 요소를 업데이트하는 데 사용할 수 있습니다.

3. **완료 콜백**:
    
    - `AsyncOperation` 객체에는 장면 로드가 완료되면 `true`가 되는 `isDone` 속성이 있습니다.
    - 이를 `Update` 메소드에서 확인하거나 `AsyncOperation.completed` 이벤트를 사용하여 장면 로딩이 완료된 직후 코드를 실행할 수 있습니다.

4. **장면 활성화**:
    
    - 기본적으로 새 장면은 완전히 로드되면 자동으로 활성화(렌더링 및 실행)됩니다.
    - `AsyncOperation.allowSceneActivation`을 `false`로 설정하여 이 동작을 변경할 수 있습니다. 이를 통해 준비가 될 때까지 활성화를 지연할 수 있습니다(장면을 미리 로드하거나 장면 전환을 동기화하는 데 유용함).

5. **사용 사례**:
    
    - 외관상의 로딩 시간을 줄여 사용자 경험을 개선하기 위해 장면이 큰 게임에 이상적입니다.
    - 일반적으로 다음 장면이 로드되는 동안 진행률 표시줄이나 애니메이션을 표시하는 로딩 화면에 사용됩니다.



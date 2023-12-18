---
layout: post
title: Project Trouble Shooting
subtitle: Static 과 초기화
author: Daniel
categories: Unity
tags: 
 - Unity
 - Project
banner:
  image: https://i.imgur.com/EiIRgWU.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

현재 Scene이 Null이 발생하는 문제
--

- CurrentScene에 `IntroScene`을 할당하고 CurrentScene 이 IntroScene 인것을 확인

```csharp
protected override bool Initialized()  
{  
    if (!base.Initialized()) return false;  
    // TODO : 인트로 씬 실행시 Context 작성   
    CurrentScene = Label.IntroScene;  
    LoadResource();  
    return true;  
}  
  
private void LoadResource()  
{  
    if (Main.Resource.LoadIntro)  
    {  
        // TODO : 로드가 되어있다면, 추가적인 초기화 필요  
        Main.UI.SetSceneUI<Intro_UI>();  
    }  
    else  
    {  
        string sceneType = CurrentScene.ToString();  
        Debug.Log($"Current Scene : {sceneType}");  
        Main.Resource.AllLoadAsync<UnityEngine.Object>($"{sceneType}", (key, count, totalCount) =>  
        {  
            Debug.Log($"[{sceneType}] Load asset {key} ({count}/{totalCount})");  
            if (count < totalCount) return;  
            Main.Resource.LoadIntro = true;  
            // TODO : 추가적인 초기화 필요  
            Main.UI.SetSceneUI<Intro_UI>();  
        });    
	}
}
```

![](https://i.imgur.com/EiIRgWU.jpg)

- 하지만 Popup을 열어 Time Scale을 확인하려고 하는데 Null 이 발생하는 문제

```csharp
private void SetTimeScale()  
{  
    Debug.Log($"UI_Manager - CurrentScene : {Main.Scene.CurrentScene}");  
    if (Main.Scene.CurrentScene != Label.GameScene)  
    {        
	    Time.timeScale = 1;  
        return;  
    }    
    Time.timeScale = _popupOrder.Count > 0 ? 0 : 1;  
}
```

### 문제확인

- private 로 선언된 변수를 public으로 변화시켜 인스펙서에서 실시간으로 해당 변수의 변화를 확인 시도

```csharp
public Label _sceneLabel;  

public Label CurrentScene  
{  
    get  
    {  
        Debug.Log($"Get Label {_sceneLabel.ToString()}");  
        return _sceneLabel;  
    }    
    
    set  
    {  
        _sceneLabel = value;  
        Debug.Log($"Set Label {_sceneLabel.ToString()}");  
    }
}
```

![](https://i.imgur.com/JTlqICf.jpg)

![](https://i.imgur.com/TfWEtko.jpg)

- IntroScene에서는 정상적으로 할당이 되나, Singleton 에서 Scene이 정상적으로 할당 되지 않는 문제 확인

- 이것은 Main 스크립트에서 초기화가 제대로 적용되지 않는 것으로 보임

```csharp
#region Fields  
  
private readonly UI_Manager _ui = new();  
private readonly ResourceManager _resource = new();  
private readonly PlayerController _playerControl = new();  
private readonly DataManager _data = new();  
public BaseScene _scene;   
#endregion  
  
#region Properties  
  
public static UI_Manager UI => Instance._ui;  
public static ResourceManager Resource => Instance._resource;  
public static BaseScene Scene => Instance._scene;  
  
#endregion
```


### 문제 해결

- Scene 할당시 Main의 씬 객체에 할당

```csharp
public static void SetCurrentScene(BaseScene scene, Label sceneLabel)  
{  
    Instance._scene = scene;  
    Instance._scene.CurrentScene = sceneLabel;  
}
```

- 각 씬 호출 마다 해당 메서드를 실행

```csharp
public class IntroScene : BaseScene  
{  
    protected override bool Initialized()  
    {        
	    if (!base.Initialized()) return false;  
        // TODO : 인트로 씬 실행시 Context 작성   
		Main.SetCurrentScene(this, Label.IntroScene);  
        LoadResource();  
        return true;  
    }
}
```



이벤트 실행 문제
--

- 이벤트가 마지막 할당한 Key 값만 적용되어 모든 `Click`이 한가지 동작만 되는 문제

```csharp
protected override bool Initialized()  
{  
    if (!base.Initialized()) return false;  
	SetButton(typeof(Buttons));  
	GetButton((int)Buttons.StartBtn).gameObject.SetEvent(UIEventType.Click, StartGame);  
    GetButton((int)Buttons.ContinueBtn).gameObject.SetEvent(UIEventType.Click,StartGame);  
    GetButton((int)Buttons.OptionBtn).gameObject.SetEvent(UIEventType.Click,OptionOpen);  
    GetButton((int)Buttons.ExitBtn).gameObject.SetEvent(UIEventType.Click, ShutdownGame);  
    return true;  
}
```

```csharp
public enum UIEventType  
{  
    Click, PointerEnter, PointerExit  
}  

public class UI_EventHandler : MonoBehaviour, IPointerClickHandler, IPointerEnterHandler, IPointerExitHandler  
{  
    private static Dictionary<UIEventType, Action<PointerEventData>> EventHandlers = new();  
  
    private void InvokeEventAction(UIEventType eventType, PointerEventData eventData)  
    {        
	    if (EventHandlers.TryGetValue(eventType, out var action)) action?.Invoke(eventData);  
    }  
    
    public void BindEvent(UIEventType eventType, Action<PointerEventData> action)  
    {        
	    EventHandlers[eventType] = action;  
    }  
    
    public void UnbindEvent(UIEventType eventType)  
    {        
	    if (EventHandlers.ContainsKey(eventType))  
        {  
            EventHandlers.Remove(eventType);  
        }  
    }  
  
    public void OnPointerClick(PointerEventData eventData) => InvokeEventAction(Click, eventData);  
    public void OnPointerEnter(PointerEventData eventData) => InvokeEventAction(PointerEnter, eventData);  
    public void OnPointerExit(PointerEventData eventData) => InvokeEventAction(PointerExit, eventData);  
  
    private void OnDestroy()  
    {        
	    EventHandlers.Clear();  
    }
}
```

- 각 오브젝트에 해당 클래스를 이벤트 오브젝트마다 할당하여 사용
- 그런데 왜 모든 오브젝트의 이벤트가 같은 기능을 작동하는지 문제 파악 필요

```csharp
protected override bool Initialized()  
{  
    if (!base.Initialized()) return false;  
    SetButton(typeof(Buttons));  
    GetButton((int)Buttons.StartBtn).gameObject.SetEvent(UIEventType.Click, StartGame);  
    GetButton((int)Buttons.ContinueBtn).gameObject.SetEvent(UIEventType.Click,StartGame);  
    GetButton((int)Buttons.OptionBtn).gameObject.SetEvent(UIEventType.Click,OptionOpen);  
    GetButton((int)Buttons.ExitBtn).gameObject.SetEvent(UIEventType.Click, ShutdownGame);  
    return true;  
}  
  
private void OptionOpen(PointerEventData obj)  
{  
    Main.UI.OpenPopup<Option_Popup>();  
}  
  
private void StartGame(PointerEventData obj)  
{  
    SceneUtility.LoadScene("GameScene");  
}  
  
private void ShutdownGame(PointerEventData obj)  
{  
    Application.Quit();  
}
```

- 어떠한 버튼을 눌러도 마지막에 이벤트 등록된 ShutDownGame이 실행 됨

![](https://i.imgur.com/5KERJaj.jpg)

- `OptionBtn`을 실행 했을 때 Intro_UI.ShutdownGame() 이 실행되는 Debug 모습

### 문제 해결

- 간단한 문제 였지만 찾는데 오래 걸렸다
- UI_EventHandler 의 키를 저장하는 Dictionary 가 `static`으로 선언
- 그로인해 모든 이벤트가 마지막 세팅된 이벤트를 따가가게 되는 현상

![](https://i.imgur.com/6TjS1UH.jpg)

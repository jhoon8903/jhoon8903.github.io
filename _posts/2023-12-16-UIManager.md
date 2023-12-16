---
layout: post
title: Scene UI Setting
subtitle: UI Manager 에서 UI Scene UI Setup
author: Daniel
categories: Unity
tags: 
 - Unity
 - Programming
banner:
  image: https://i.imgur.com/DxT7PYi.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

UI Canvas Setting
--

- UI 오브젝트를 인스턴화 할때 아용하는 방법입니다.
- 협업시 충돌을 줄이기 위해서 모든 인스턴스는 프리팹화 시키며, 이를 부를때 UI 속성을 가지고 있는 오브젝트는 꼭 Canvas가 있어야 하기 때문에 프리팹에 추가 하거나 미리 생성된 Canvas의 하위 오브젝트로 넣어야 정상적으로 사용이 가능합니다.

### 씬 오브젝트 인스턴스화

```csharp
public void SetSceneUI<T>() where T : UI_Base  
{  
    string sceneTypeName = typeof(T).Name;  
    T sceneUI = SetUI<T>(sceneTypeName);  
    sceneUI.transform.SetParent(UIBase.transform);  
    
    if (Enum.TryParse<SceneUtility.SceneName>(sceneTypeName, out var sceneEnum))  
    {        
	    Main.Scene.CurrentScene = sceneEnum;  
    }
}
```

- UI_Base를 상속받는 오브젝트만 사용이 가능하며, `typeof(T).Name`을 이용하여 Scene의 이름을 받아올 수 있습니다.

```csharp
private T SetUI<T>(string uiName) where T : Component  
{       
// TODO : 리소스 매니져 생성 시 수정 필요  
    GameObject uiObject = null;  
    // GameObject uiObject = Main.Resource.InstantiatePrefab(${name}.prefab);  
    switch (uiName)  
    {  
        case "Option_Popup":  
        {  
            uiObject = Instantiate(Main.Scene.optionPopup);  
            break;  
        }        
        case "Intro_UI":  
        {  
            uiObject = Instantiate(Main.Scene.introUI);  
            break;  
        }    
	}    
	return uiObject.GetOrAddComponent<T>();  
}
```

- 리소스 매니져가 아직 개발 되지 않아 임시 프리팹으로 대체
- 인스턴스화 된 GameObject에 `GetOrAddCompoenent<T>()`를 사용하여 만약 오브젝트에 해당 클래스의 스크립트 컴포넌트가 없다면 할당이 가능해진다

#### GetOrAddCompoenet<`T`>();

```csharp
namespace Unity.VisualScripting  
{
	public static class ComponentHolderProtocol  
	{
		public static T GetOrAddComponent<T>(this UnityObject uo) where T : Component  
		{  
		    return uo.GetComponent<T>() ?? uo.AddComponent<T>();  
		}
	}
}
```

- `ComponentHolderProtocol` 클래스에서 제공하는 메서드 이며, 해당 오브젝트의 컴포넌트가 있다면 컴포넌트를 반환, 만약 없다면 `T` 컴포넌트를 Add 해주는 기능 
- VisualScripting NameSpace 그룹의 메서드입니다.
- 하지만 UnityEngine 에서는 사용이 안되기 때문에 별도의 비슷한 메서드를 만들어 줍니다.

```csharp
public static T GetAddComponent<T>(UnityEngine.Object obj) where T : Component  
{  
    return obj.GetComponent<T>() ?? obj.AddComponent<T>();  
}
```

```csharp
public void OrderLayerToCanvas(GameObject uiObject, bool sort = true)  
{  
    Canvas canvas = uiObject.GetOrAddComponent<Canvas>();  
    canvas.renderMode = RenderMode.ScreenSpaceOverlay;  
    canvas.overrideSorting = true;  
    SortingOder(canvas,sort);  
    CanvasScaler scales = canvas.GetOrAddComponent<CanvasScaler>();  
    scales.uiScaleMode = CanvasScaler.ScaleMode.ScaleWithScreenSize;  
    scales.referenceResolution = new Vector2(1920, 1080);  
    canvas.referencePixelsPerUnit = 100;  
}  
  
private void SortingOder(Canvas canvas, bool sort)  
{  
    if (sort)  
    {        
	    canvas.sortingOrder = _sortByOrderLayer;  
        _sortByOrderLayer++;  
    }    
    else  
    {  
        canvas.sortingOrder = 0;  
    }
}
```

- 필요에 따라서는 ScaleMode, 해상도, sortOrder 를 조정하여 UI Layer 구성이 가능합니다.
---
layout: post
title: 피드백에 따른 UI Manager 리팩토링
subtitle: UI Manager Refactoring
author: Daniel
categories: Unity
tags: 
 - Unity
 - Programming
banner:
  image: https://i.imgur.com/41pKEtw.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

UI Manager 관련 피드백
--

> 1. SetBinder 클래스에서 Binding 메서드를 제너릭 메서드로 만들고, UI 컴포넌트 타입을 직접 전달하여 해당 타입에 대한 바인딩만 수행하도록 변경하면 좋을듯합니다. 그럼 코드의 재사용성이 높아지겠죠?
> 
> 2.  UI_Base 클래스에서 GetUIComponents 메서드를 각 UI 컴포넌트 타입에 따라 별도의 메서드를 만들지 않고, 단일 제너릭 메서드로 통합하면 코드 중복이 줄어들꺼에요.
> 
> 3.  SetBinder 클래스에서 FindComponentRecursive 메서드가 재귀적인 방식을 따르는데, 이건 대규모 오브젝트 트리에서는 굉장히 성능저하를 불러일으키게 되요. 그래서 차라리 다른 탐색 알고리즘으로 바꾸거나, 필요한 구성 요소를 캐싱하는 그런 방법들이 존재합니다. 한번 검색해보시고 적용해보면 좋겠네요.


SetBinder Class Binding 메서드 제네릭
--

### 기존 코드

```csharp
public class SetBinder  
{  
    public static void Binding<T>(GameObject parentObject, Type type, Dictionary<Type, Object[]> objectDictionary) where T : Object 
    {  
        string[] names = Enum.GetNames(type);  
        Object[] objects = new Object[names.Length];  
        objectDictionary.Add(typeof(T), objects);  
        AssignmentComponent<T>(parentObject, names, objects);  
    }
}  
```

```csharp
public class UI_Base : MonoBehaviour
{
	#region Bind  
	  
	private void BindUIComponents<T>(Type enumType) where T : Object  
	{  
	    SetBinder.Binding<T>(gameObject, enumType, _objects);  
	}  
	  
	protected void SetButton(Type type) => BindUIComponents<Button>(type);  
	protected void SetImage(Type type) => BindUIComponents<Image>(type);  
	protected void SetText(Type type) => BindUIComponents<TextMeshProUGUI>(type);  
	protected void SetObject(Type type) => BindUIComponents<GameObject>(type);  
	  
	#endregion

	#region Getter  
	  
	private T GetUIComponents<T>(int componentIndex) where T : Object  
	{  
	    return SetBinder.Getter<T>(componentIndex, _objects);  
	}  
	  
	protected Button GetButton(int componentIndex)  
	{  
	    return GetUIComponents<Button>(componentIndex);  
	}  
	  
	protected Image GetImage(int componentIndex)  
	{  
	    return GetUIComponents<Image>(componentIndex);  
	}  
	  
	protected TextMeshProUGUI GetText(int componentIndex)  
	{  
	    return GetUIComponents<TextMeshProUGUI>(componentIndex);  
	}  
	  
	protected GameObject GetObject(int componentsIndex)  
	{  
	    return GetUIComponents<GameObject>(componentsIndex);  
	}  
	  
	#endregion
}
```

- 해당 호출 방법을 Base를 지나 전달이 되는데 직접 전달하는 방법으로 변경


### 수정 코드

#### Binder Class

```csharp
private readonly Dictionary<Type, Dictionary<string, Object>> _objects = new();  

public void Binding<T>(GameObject parentObject) where T : Object  
{  
    T[] objects = parentObject.GetComponentsInChildren<T>(true);  
    
    Dictionary<string, Object> objectDict = objects.ToDictionary(comp => comp.name, comp => comp as Object);  
    
    _objects[typeof(T)] = objectDict;  
    
    AssignmentComponent<T>(parentObject, objectDict);  
}
```

- 별도의 지정된  enumType을 검사하는 것이 아닌, 오브젝트의 타입을 검사하므로서 재사용성을 높힘

#### UI Base Class

```csharp
public class UIBase : MonoBehaviour  
{  
    private Binder _binder;  
    
    private void Start()  
    {        
	    _binder = ServiceLocator.GetService<Binder>();  
    }    
    
    protected void SetUI<T>() where T : Object  
    {  
        _binder.Binding<T>(gameObject);  
    }    
    
	protected T GetUI<T>(string componentName) where T : Object  
	{  
	    return _binder.Getter<T>(componentName);  
	}
}
```

- 불필요한 중복 코드 삭제

#### UI 호출부

```csharp
// 기존 SetUI
SetText(typeof(Texts));
AnyPressText = GetText((int)Texts.AnyPressText);

// 수정 후 Set UI
SetUI<TextMeshProUGUI>();
_startText = GetUI<TextMeshProUGUI>("StartText");
```

- 해당 타입의 컴포넌트를 조회하는 방법으로 변경

#### 변경 후 

- 해당 로직을 변경하면서 드는 의문은 2가지 였습니다.

1. 기존 방식은 특정 오브젝트를 직접 딕셔너리에 담아서 사용하기 때문에 Find 형식이 되지만, 런타임에 리플렉션으로 작동하는 부분에 있어서 성능이 하락 할 수 있는 부분이 존재함
2. 수정 방식은 리플랙션 방식이 아니지만, 전체 오브젝트를 훑어서 찾아야 하는 방식이다 보니, 만약 UI 구성이 많다면 성능하락이 야기되는 부분이 존재 

- 하지만 UI 구성 자체가 엄청 복잡하고 많은 오브젝트가 존재하지 않는다면, 성능하락 폭에 대해서는 무시할 수 있는 수준으로 보이며, 대신 코드의 재사용성을 높이는 방법이 더 좋다고 판단 됨
- 성능의 하락적인 부분에 있어서는 `캐싱`을 얼마나 잘하냐에 따라 다라질 것이기 때문에 추가적인 캐싱 로직을 프로젝트 마지막에 진행할 예정


## 재귀 매서드 
--

- 재귀 함수의 경우 프로젝트의 사이즈가 커지거나 컴포넌트들의 수가 많아진다면 분명히 문재가 될 수 있습니다.

```csharp
private static void AssignmentComponent<T>(GameObject parentObject, IReadOnlyList<string> names, IList<Object> objects) where T : Object  
{  
    for (int i = 0; i < names.Count; i++)  
    {        
	    if (typeof(T) == typeof(GameObject))  
        {            
	        objects[i] = FindComponent(parentObject, names[i], true);  
        }        
        else  
        {  
        objects[i] = FindComponent<T>(parentObject, names[i], true);  
        }        
        
        if (objects[i] == null) Debug.Log($"바인드 실패 : {names[i]}");  
    }
}  
  
private static GameObject FindComponent(GameObject parentObject, string name = null, bool recursive = false)  
{  
    Transform transform = FindComponent<Transform>(parentObject, name, recursive);  
    return transform == null ? null : transform.gameObject;  
}  
  
private static T FindComponent<T>(GameObject parentObject, string name, bool recursive) where T : Object  
{  
    if (parentObject == null) return null;  
  
    return recursive
	    ? FindComponentRecursive<T>(parentObject, name)
	    : FindComponentNonRecursive<T>(parentObject, name);  
}  
  
private static T FindComponentNonRecursive<T>(GameObject parentObject, string name) where T : Object  
{  
    for (int i = 0; i < parentObject.transform.childCount; i++)  
    {        
	    Transform child = parentObject.transform.GetChild(i);  
        
        if (!string.IsNullOrEmpty(name) && child.name != name) continue;  
        
        T component = child.GetComponent<T>();  
        
        if (component != null) return component;  
    }    
    return null;  
}  
  
private static T FindComponentRecursive<T>(GameObject parentObject, string name) where T : Object  
{  
    var components = parentObject.GetComponentsInChildren<T>();  
    return components.FirstOrDefault(component => string.IsNullOrEmpty(name) || component.name == name);  
}
```

#### 시간 복잡도

1. **`Binding<T>` 방법**:
    - `GetComponentsInChildren<T>`: O(n), 여기서 n은 하위 구성 요소 수입니다.
    - `ToDictionary`: O(n), 사전을 생성하기 위해 모든 구성요소를 반복하므로.

2. **`AssignmentComponent<T>` 메서드**:
    - 키 반복: O(k), 여기서 k는 사전에 있는 키 수입니다.
    - `FindComponent`: 각 호출은 비재귀의 경우 O(1)이고 재귀의 경우 O(m)입니다(m은 계층 구조의 총 구성 요소 수입니다).

- 여기서 전반적인 시간 복잡성은 주로 재귀 검색의 영향을 받습니다. 
- `FindComponentRecursive`를 자주 사용하고 구성 요소 계층 구조가 깊다면 시간 복잡도가 크게 증가할 수 있습니다.

#### 공간 복잡성

- 주요 공간 복잡성은 `_objects` 사전에 참조를 저장하는 데서 발생합니다. O(n)입니다. 여기서 n은 저장된 구성 요소의 총 개수입니다.

### 수정 코드

```csharp
private void AssignmentComponent<T>(GameObject parentObject, Dictionary<string, Object> objects) where T : Object  
{  
    foreach (var key in objects.Keys.ToList())  
    {        
	    if (objects[key] != null) continue;  
        Object component = typeof(T) == typeof(GameObject)
        ? FindComponentDirectChild<GameObject>(parentObject, key)
        : FindComponentDirectChild<T>(parentObject, key);  
  
        if (component != null) objects[key] = component;  
        else Debug.Log($"Binding failed for Object : {key}");  
    }
}  
  
private T FindComponentDirectChild<T>(GameObject parentObject, string name) where T : Object  
{  
    return (from Transform child in parentObject.transform 
    where child.name == name
    select child.GetComponent<T>()).FirstOrDefault();  
}
```

- 'AssignmentComponent' 메서드는 이제 구성 요소가 처음에 발견되지 않은 경우에만 사전을 업데이트합니다. 
- 이렇게 하면 `FindComponent` 호출 횟수가 줄어듭니다.
- 'FindComponentDirectChild'는 재귀적 방법 대신 사용되어 검색을 직접 하위 항목으로 제한하여 일반적인 경우 시간 복잡도를 줄입니다.

## 전체 코드 

```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using UnityEngine;  
using Object = UnityEngine.Object;  
  
namespace UI  
{  
    public class Binder  
    {  
        private readonly Dictionary<Type, Dictionary<string, Object>> _objects = new();  
        public void Binding<T>(GameObject parentObject) where T : Object  
        {  
            T[] objects = parentObject.GetComponentsInChildren<T>(true);  
            Dictionary<string, Object> objectDict = objects.ToDictionary(comp => comp.name, comp => comp as Object);  
            _objects[typeof(T)] = objectDict;  
            AssignmentComponent<T>(parentObject, objectDict);  
        }  
        
        private void AssignmentComponent<T>(GameObject parentObject, Dictionary<string, Object> objects) where T : Object  
        {  
            foreach (var key in objects.Keys.ToList())  
            {                if (objects[key] != null) continue;  
                Object component = typeof(T) == typeof(GameObject)
                ? FindComponentDirectChild<GameObject>(parentObject, key)
                : FindComponentDirectChild<T>(parentObject, key);  
  
                if (component != null) objects[key] = component;  
                else Debug.Log($"Binding failed for Object : {key}");  
            }        
		}  
        
        private T FindComponentDirectChild<T>(GameObject parentObject, string name) where T : Object  
        {  
            return (from Transform child in parentObject.transform
            where child.name == name
            select child.GetComponent<T>()).FirstOrDefault();  
        }  
        
        public T Getter<T>(string componentName) where T : Object  
        {  
            if (!_objects.TryGetValue(typeof(T), out Dictionary<string, Object> components)) return null;  
            if (components.TryGetValue(componentName, out var component)) return component as T;  
            return null;  
        }    
	}
}
```

```csharp
using UnityEngine;  
using Object = UnityEngine.Object;  
  
namespace UI  
{  
    public class UIBase : MonoBehaviour  
    {  
        private Binder _binder;  
        
        private void Start()  
        {            
	        _binder = ServiceLocator.GetService<Binder>();  
        }        
		
		protected void SetUI<T>() where T : Object  
        {  
            _binder.Binding<T>(gameObject);  
        }        
        
        protected T GetUI<T>(string componentName) where T : Object 
        {  
            return _binder.Getter<T>(componentName);  
        }    
	}
}
```
---
layout: post
title: 게임 Window Size 비율을 유지하며 창 크기 조절하기
subtitle: 사이즈 고정 하기
author: Daniel
categories: Unity
tags: 
 - Unity
 - Window
banner:
  image:
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

사이즈를 유지하며 창 크기가 유지 될 수 있도록 하기
--

![](https://i.imgur.com/zQdaHeF.jpg)

![](https://i.imgur.com/zcIfJ7z.jpg)

- 빌드 세팅에서 `Resolution and Presentation` 중
- `Fullscreen Mode`를 `Windowed`로 맞추면 창 모드로만 게임을 실행 할 수 있음

#### 창모드

![](https://i.imgur.com/aRtJG94.jpg)

#### 전체모드

![](https://i.imgur.com/0HQTiMp.jpg)

### 문제점

- 창모드에서 사이즈를 줄이려하면 사이즈 조절에 잠겨 있는 문제가 발생

![](https://i.imgur.com/mDZ5uaF.gif)

- Resizeable Window 를 체크하면 사이즈 조절은 가능해짐

![](https://i.imgur.com/HhVrlO7.jpg)

- 너무 자유로워서 개발자가 의도한 사이즈만 제공할 수 없는 문제

![](https://i.imgur.com/MI64GcZ.gif)

- 빌드 옵션이 별도로 사이즈를 고정해서 조절할 수 있는 옵션이 없어 스크립트로 작성하여
- 엔진에서 사이즈를 확인하여 고정할 수 있도록 합니다.

### 스크립트

#### 해상도 컨트롤러

```csharp
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
  
public class ResolutionController : MonoBehaviour  
{  
    private const float TargetAspectRatio = 9f / 19.5f;  
    private Vector2 _lastScreenSize;  
    private Coroutine _resizeCoroutine;  
    private const float DebounceTime = 0.1f;  
  
    private void Start()  
    {        
	    _lastScreenSize = new Vector2(Screen.width, Screen.height);
	    DontDestroyOnLoad(gameObject);  
    }  
    private void Update()  
    {        
	    Vector2 screenSize = new Vector2(Screen.width,
	    Screen.height);  
  
        if (screenSize == _lastScreenSize) return;  
        if (_resizeCoroutine != null)
        StopCoroutine(_resizeCoroutine);  
  
        _resizeCoroutine =
        StartCoroutine(DebounceResize(screenSize));  
        _lastScreenSize = screenSize;  
    }  
    
    private IEnumerator DebounceResize(Vector2 newSize)  
    {        
	    yield return new WaitForSeconds(DebounceTime);  
  
        float currentAspectRatio = newSize.x / newSize.y;  
  
        if (currentAspectRatio > TargetAspectRatio)  
        {            
	        int newWidth = Mathf.RoundToInt(newSize.y *
	        TargetAspectRatio);  
  
            if (newWidth > 0 && newSize.y > 0)  
            {                
	            Screen.SetResolution(newWidth, (int)newSize.y,
	            false);  
            }        
		}        
		else  
        {  
            int newHeight = Mathf.RoundToInt(newSize.x /
            TargetAspectRatio);  
  
            if (newSize.x > 0 && newHeight > 0)  
            {                
	            Screen.SetResolution((int)newSize.x, newHeight,
	            false);  
            }        
		}    
	}
}
```

#### 디바운스 매커니즘 (Debounce Mechanism)

> 디바운스 메커니즘은 함수가 너무 자주 호출되지 않도록 하는 데 사용되는 프로그래밍 기술입니다.
>  
> 이는 창 크기 조정, 검색 상자에 입력 또는 버튼 클릭 처리와 같이 짧은 기간 동안 이벤트가 빠르고 반복적으로 트리거될 수 있는 시나리오에서 특히 유용할 수 있습니다. 
> 
> 디바운싱의 목표는 함수가 실행되는 속도를 제한하여 성능을 향상하고 불필요한 처리를 줄이는 것입니다.

#### 해당 스크립트에서 작동하는 디바운싱 방식

1. **트리거 이벤트**: 일반적으로 함수 실행을 트리거하는 이벤트가 발생합니다(예: 사용자가 창 크기를 조정함).
2. **타이머 시작**: 기능을 즉시 실행하는 대신 타이머가 시작됩니다. 타이머가 만료되기 전에 이벤트가 다시 트리거되면 타이머가 재설정됩니다.
3. **함수 실행**: 함수는 타이머가 만료되고 해당 시간 내에 새로운 트리거 이벤트가 발생하지 않은 후에만 실행됩니다.

#### 코루틴을 중단하지 않고 지속적으로 함수를 호출하면 발생하는 문제

- 창 크기가 조정될 때마다 발생하는 이벤트가 있다고 가정해 보세요. 
- 디바운싱이 없으면 창 크기가 조정되는 동안 레이아웃을 조정하는 함수가 여러 번 호출되어 잠재적으로 성능 문제가 발생할 수 있습니다. 
- 디바운싱을 사용하면 사용자가 창 크기 조정을 중지한 후에 이 함수가 한 번만 호출되므로 호출 수가 줄어들고 성능이 향상됩니다.
#### 비용 최적화

- 디바운스 메커니즘은 창 크기 조정 빈도를 제어하는 ​​데 사용됩니다. 
- 창 크기가 변경되면 종횡비를 즉시 조정하는 대신 코루틴이 지연(`debounceTime`)으로 시작됩니다. 
- 지연이 끝나기 전에 창 크기가 다시 변경되면 코루틴이 중지되고 다시 시작됩니다. 
- 이렇게 하면 크기 조정이 잠시 동안 비활성화된 후에만 가로세로 비율 조정이 이루어지므로 잠재적으로 비용이 많이 드는 `Screen.SetResolution` 호출의 빈도가 줄어듭니다.


![](https://i.imgur.com/EeyrWOt.gif)

### 추가적인 문제점

#### 최소 사이즈 설정

- 최소 사이즈를 지정하지 않아 한업이 작아지는 문제 발생
- 최소 사이즈는 빌드 사이즈의 50% 수준으로 설정

#### 가로 세로 별도로 사이즈 조정이 가능해져 사이즈 변경 시 빈 공간 노출

- 이 부분은 코루틴을 호출 하는 과정에서 오는 딜레이 트러블
- 게임 화면 자체의 공간을 넓혀 백그라운드를 감추는 방법과 코루틴이 아닌 업데이트문에서 실시간으로 사이즈를 확인 하면 되지만, 2안의 경우 리소스 소모가 많아져 차라리 전체 사이즈에 대응이 가능한 백그라운드를 채우는 방법을 선택 
- 다만, 이 문제는 PC 에서만 발생하기 때문에 사이즈를 조정할 수 없는 모바일 환경에서는 아무런 문제가 되지 않습니다.

#### 최소사이즈 유지 스크립트 수정

```csharp
// 최소 가로 / 세로 픽셀 사이즈
private const float MinWidth = 540f;  
private const float MinHeight = 1170;

// 디바운스 (동시 변경 및 최소 사이즈 적용)
private IEnumerator DebounceResize(Vector2 newSize)  
{  
    yield return new WaitForSeconds(DebounceTime);  
  
    bool isWidthResized = !Mathf.Approximately(newSize.x,
    _lastScreenSize.x);  
  
    if (isWidthResized)  
    {        
	    int newHeight = Mathf.RoundToInt(newSize.x /
	    TargetAspectRatio);  
        
        if(newHeight < MinHeight)  
        {            
	        newHeight = (int)MinHeight;  
            newSize.x = Mathf.RoundToInt(newHeight *
            TargetAspectRatio);  
        }        
        Screen.SetResolution((int)newSize.x, newHeight, false);  
    }    
    else  
    {  
        int newWidth = Mathf.RoundToInt(newSize.y *
        TargetAspectRatio);  

		if(newWidth < MinWidth)  
        {            
	        newWidth = (int)MinWidth;  
            newSize.y = Mathf.RoundToInt(newWidth /
            TargetAspectRatio);  
        }        
        Screen.SetResolution(newWidth, (int)newSize.y, false);  
    }
}
```


![](https://i.imgur.com/6JhnLCE.gif)

---
layout: post
title: IPointerHandler 이벤트
subtitle: 이벤트 유형 및 예제에 대해 알아보자
author: Daniel
categories: Unity
tags: 
 - Unity
 - Event
 - Programming
banner:
  image: https://doc.qt.io/qt-6/images/pointerhandlers-example-joystick.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

IPointerHandler
--

- EventSystem namespace의 일부
- 주로 마우스나 터치 스크린과 같은 포인팅 장치의 입력을 처리하는데 사용
- 여러 유형의 포인터 이벤트가 있으며, 각각은 다양한 형태의 사용자 상호작용에 응답하도록 설계 됨

### IPointerHandler 이벤트 유형

- IPointerEnterHandler : 포인터가 UI 요소의 `영역에 들어갈 때` 트리거
- IPointerExitHandler : 포인터가 UI 요소의 `영역을 벗어날 때` 트리거
- IPointerDownHandler : UI 요소위에 `포인터를 눌렀을 때` 트리거
- IPointerUpHandler : UI 요소위에 `포인터를 (버튼 위) 놓으면` 트리거
- IPointerClickHandler : UI 요소에서 `클릭이 감지되면` 트리거

### 반응 예제

- `IPointerEnterHandler` 및 `IPointerExitHandler`의 경우, 마우스를 버튼 위에 올리면 색상이 변경 되는 등에 상호작용 이벤트에 사용 됨
- `IPointerDownHandler` 및 `IPointerUpHandler`의 경우에는, 사용자가 마우스 및 터치를 누르거나 놓을 때 개체를 끌거나 할때 사용되며, 대화형 요소 및 아이템 드래그 등에 사용
- `IPointerClickHandler` 클릭동작으로 활성화되는 버튼 및 대화형 요소에 사용

### 인터페이스 상속

```csharp
public class UI_EventHandler : MonoBehaviour, IPointerClickHandler, IPointerEnterHandler, IPointerExitHandler  
{
	public void OnPointerClick(PointerEventData eventData) => InvokeEventAction(Click, eventData);  
	public void OnPointerEnter(PointerEventData eventData) => InvokeEventAction(PointerEnter, eventData);  
	public void OnPointerExit(PointerEventData eventData) => InvokeEventAction(PointerExit, eventData);
}
```

#### 사용예제 
- 마우스 호버가 되었을 때 Tooltip을 띄우는 이벤트 예제

```csharp
using UnityEngine; 
using UnityEngine.EventSystems; 

public class TooltipHandler : MonoBehaviour, IPointerEnterHandler, IPointerExitHandler 
{ 
	public GameObject tooltip;
	
	public void OnPointerEnter(PointerEventData eventData) 
	{ 
		tooltip.SetActive(true);
	} 
	
	public void OnPointerExit(PointerEventData eventData) 
	{ 
		tooltip.SetActive(false);
	} 
}
```
#### PointerEventData

- 마우스나 터치 등 포인터 입력 이벤트에 대한 정보를 포함하는 Unity Class

#### 요소

- **pointerId**: 포인터를 식별합니다. 마우스의 경우 일반적으로 왼쪽 버튼은 '-1', 오른쪽 버튼은 '-2', 가운데 버튼은 '-3'입니다. 터치의 경우 터치의 손가락 ID입니다.
- **position**: 포인터의 현재 화면 위치입니다.
- **delta**: 마지막 프레임 이후 포인터 위치의 차이입니다.
-  **pressPosition**: 포인터를 누른 화면 위치입니다.
-  **clickTime**: 마지막 클릭이 발생한 시간입니다.
-  **clickCount**: 짧은 순서의 클릭 수(더블 클릭 등을 감지하는 데 유용함).
-  **scrollDelta**: 마우스 휠의 스크롤 델타입니다(해당되는 경우).
-  **useDragThreshold**: 드래그 임계값을 확인할지 여부를 나타내는 부울입니다.
-  **dragging**: 포인터가 현재 개체를 드래그하고 있는지 여부를 나타냅니다.
- **button**: 이벤트와 관련된 마우스 버튼(왼쪽, 오른쪽, 가운데).
- **eligibleForClick**: 포인터가 클릭 이벤트에 적합한 것으로 간주되는지 여부를 나타냅니다.
- **pointerDrag**: OnDrag 이벤트를 수신한 마지막 GameObject입니다.
- **pointerPress**: OnPointerDown 이벤트를 수신한 GameObject입니다.
- **rawPointerPress**: 잠재적인 포인터 리디렉션 전에 OnPointerDown 이벤트를 수신한 원시 GameObject입니다.
- **pointerCurrentRaycast**: 적중된 GameObject, 적중까지의 거리, 적중의 월드 위치 및 노멀, 레이캐스트 결과의 결과 인덱스 등 포인터가 현재 위에 있는 것에 대한 정보를 포함합니다. 목록.
- **pointerEnter**: 가장 최근의 OnPointerEnter 이벤트 중에 포인터가 들어간 GameObject입니다.
- **lastPress**: OnPointerDown 이벤트를 수신한 마지막 GameObject입니다(클릭 감지에 유용함).
- **hovered**: 포인터가 현재 가리키고 있는 GameObject의 목록입니다.
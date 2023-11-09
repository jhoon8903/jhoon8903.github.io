---
layout: post
title: Unity Trail Renderer
subtitle: 총알의 궤적이나 리소스 사용을 줄이는 Trail 렌더링
categories: Unity
author: Daniel
tags: 
 - Unity
 - Rendering
banner:
 image: https://i.imgur.com/1VgNyO5.gif
---

![](https://i.imgur.com/1VgNyO5.gif)

Trail Renderer
--

- 오브젝트의 궤적을 표시하는 컴포넌트 
- 오브젝트 선택 => Component => Effects => Trail Renderer

![](https://i.imgur.com/Hk95cXQ.jpg)


## Trail Renderer Component

![](https://i.imgur.com/5HXdgCV.jpg)
### Width (궤적 너비) - float
- 시작점과 끝 점을 조정하여 Trail의 너비를 설정 할 수 있음

---

### Time (지속 시간) - float
- 궤적의 유지 시간 설정 
- 5.0으로 설정 시 Trail은 5초간 유지

---

### Min Vertex Distance (최소 Vertex 거리) - float
- 새로운 궤적 포인트가 생성되기 전 오브젝트가 이동해야 하는 최소 거리 
- 0.1로 설정하면 0.1 유닛마다 새로운 포인트가 생성 됨
#### ❓Vertex
	- 3D 모델에서의 단일 점
	- X, Y, Z 좌표값을 가지며, 여러 버텍스(Vertices)가 연결되어 'Polygon', 'Mesh'를 생성
	- 색상, 텍스처 좌표(UV), 법선 벡터 등 데이터 정보를 담고 있음 

---

### Autodestruct (자동 파괴) - bool
- Trail이 더 이상 Rendering 되지 않을 때 해당 게임 오브젝트를 자동 파괴 할 지의 여부

---

### Emitting (방출) - bool
- Trail 표시 여부
- false 일때 Trail이 표시 되지 않음

---

### Apply Active Color Space (활성 색상 공간 적용) - bool
- Liner와 Gamma 색상 공간 간 변환을 자동으로 처리할 지 결정

---

### Color (색상) - Gradient
- Trail의 색상을 설정

---

### Corner Vertices (모서리 버텍스) - integer
- 궤적의 모서리에서 버텍스 수를 조절

---

### End Cap Vertices (끝 버텍스) - integer
- 끝 지점의 버텍스 수를 조절

---

### Alignment (정렬) - enum
- 궤적의 정렬 형태 설정
##### View
- Trail 은 카메라 View에 맞춰서 수직으로 그려짐
- Trail이 항상 View Point를 향하도록 함
- 어떤 방향에서 Trail이 생성되어도 View 맞추어 일정하게 생성
##### Transform z
- Trail이 항상 Transform z 에 맞춰서 그려짐
- Trail의 방향성은 Local 좌표를 기준으로 함
- 오브젝트의 z 축이 특정 방향을 향하고 있다면, Trail도 그 방향을 기준으로 형성
- 즉 방향이 바뀌면 Trail의 방향도 바뀜

---

### Texture Mode (텍스처 모드) - enum
- Trail에 Texture Mapping 설정
##### Stretch
    - 텍스처는 궤적의 전체 길이에 걸쳐서 늘려져(stretched) 매핑됩니다. 
    - 즉, 궤적이 길어지거나 짧아지면 텍스처의 크기 또한 그에 따라 늘어나거나 줄어듭니다.
##### Tile
    - 텍스처는 궤적의 길이에 상관없이 일정한 크기로 반복(tiled)해서 나타납니다. 
    - 궤적이 길어지면 텍스처 패턴이 반복되어 더 많이 나타나게 됩니다.
##### Distribute Per Segment
    - 각 궤적 세그먼트마다 텍스처가 균일하게 분배됩니다. 
    - 텍스처는 각 세그먼트의 시작부터 끝까지 매핑됩니다.
##### Repeat Per Segment
    - 텍스처는 각 궤적 세그먼트마다 반복적으로 나타납니다. 
    - 즉, 각 세그먼트에 동일한 텍스처 패턴이 여러 번 나타날 수 있습니다.
##### Static
    - 텍스처는 궤적의 전체 길이에 대해 고정된 위치에 매핑됩니다. 
    - 궤적의 길이나 형태가 변하더라도 텍스처의 매핑 위치는 변하지 않습니다.

#### ❓ 세그먼트 (Segment)
- Vertex와 Vertex 사이의 직선
- 세그먼트는 궤적이 움직임에 따라 시간의 흐름에 따라 생기는 연속적인 부분들을 의미
- 객체가 움직이면서 새로운 세그먼트가 추가되고, 오래된 세그먼트는 사라지게 됩니다.

---

### Texture Scale (텍스처 스케일) - float
- Trail의 Scale을 조정

---

### Generate Lighting Data (조명 데이터 생성) - bool
- Lighting Data를 Trail에 적용할지에 대한 여부

---

### Shadow Bias (그림자 편향) - float
- Trail의 그림자 값을 설정

---

### Mask Interaction (Mask 상호작용) - enum
- Sprite Mask와의 상호작용 설정
##### None
- `Sprite Mask`의 존재에 상관없이 렌더링 됩니다. 
- 즉, 마스크는 이 렌더러에 영향을 주지 않습니다.
##### Visible Inside Mask
- 오직 `Sprite Mask` 내부에서만 렌더링 대상이 보여집니다. 
- 마스크의 외부 영역에서는 렌더링 대상이 보이지 않게 됩니다.
- 예시: 캐릭터가 특정 영역 내에서만 보여야 할 때 사용될 수 있습니다. 해당 영역 외에서는 캐릭터가 렌더링되지 않게 됩니다.
##### Visible Outside Mask
- `Sprite Mask`의 외부에서만 렌더링 대상이 보여집니다. 
- 마스크 내부 영역에서는 렌더링 대상이 보이지 않게 됩니다.
- 예시: 어떤 객체나 캐릭터가 특정 영역에 들어가면 사라지게 하고 싶을 때 사용할 수 있습니다.

---

### Lighting
[[2023-09-27-Shadow]]


---

### Probes
##### Light Probes (라이트 프로브)
    - 정의: 라이트 프로브는 3D 환경의 특정 지점에서의 조명 정보를 저장합니다.
    - 용도: 동적 오브젝트들에 대한 간접 조명 정보를 제공합니다. 
			    이를 통해 이동하는 오브젝트들이 주변 환경의 빛에 자연스럽게 반응하도록 할 수 있습니다.
    - 작동 방식: 라이트 프로브 그룹을 사용하여 장면에 여러 라이트 프로브를 배치하고, 
						이러한 프로브의 위치에 따른 간접 조명 정보를 계산하여 저장합니다. 
						동적 오브젝트는 가장 가까운 라이트 프로브들의 정보를 기반으로 간접 조명을 받게 됩니다.
##### Reflection Probes (리플렉션 프로브)
    - 정의: 리플렉션 프로브는 3D 환경의 특정 지점에서의 환경 반사 정보를 캡처하고 저장합니다.
    - 용도: 오브젝트들에 환경의 반사를 제공하여 재질이 메탈릭 또는 반사적인 속성을 가진 경우, 
			    주변 환경의 반사를 자연스럽게 표현할 수 있습니다.
    - 작동 방식: 리플렉션 프로브는 주변 환경을 360도로 캡처하여 큐브맵 형식으로 저장합니다. 
					    이 정보는 그 지점 근처의 오브젝트들에 반사 효과를 적용하는 데 사용됩니다.

---

### Additional Setting 
#### Motion Vectors 
#### Dynamic Occlusion 
#### Sorting Layer 
#### Order in Layer

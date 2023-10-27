---
title: Unity Scene 관리
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/gQVaaNN.jpg
comments_disable: true
meta_description: Unity Scene 관리
---

# SceneManager Class
- 동적으로 Scene을 생성, 해제, 호출 등 여러 Method를 제공

|Method|설명|
|:--|:--|
|CreateScene|새로운 빈 씬을 생성|
|LoadScene|씬의 이름 또는 인덱스 번호로 씬을 로드|
|LoadSceneAsync|씬을 비동기 방식으로 로드|
|MergeScenes|소스 씬을 다른 씬으로 통합<br>소스 씬은 모든 게임오브젝트가 통합된 이후 `삭제`|
|MoveGameObjectToScene|현재 씬에 있는 특정 게임오브젝트를 다른 씬으로 이동|
|UnloadScene|현재 씬에 있는 모든 게임오브젝트를 삭제|

## Multi Scene Load
- 여러 씬을 동시에 로딩하고 활성화하는 기능이
- `SceneManager.LoadScene`의 두 번째 파라미터로 `LoadSceneMode.Additive`를 사용
- 이렇게 하면 새로운 씬이 현재 씬 위에 추가되어 두 씬이 동시에 활성화


![](https://i.imgur.com/gQVaaNN.jpg)

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class MultiSceneLoader : MonoBehaviour
{
    void Start()
    {
        // 현재 씬을 유지한 채로 추가적으로 다른 씬을 로딩합니다.
        SceneManager.LoadScene("AnotherScene", LoadSceneMode.Additive);
    }
}
```

- LoadSceneMode.Single : 기존에 로드된 씬을 모드 삭제한 후 새로운 씬을 로드
- LoadSceneMode.Additive : 기존 씬을 삭제하지 않고 추가해서 새로운 씬을 로드




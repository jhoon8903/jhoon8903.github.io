---
title: Mini Game Refactoring - StartScene
author: "이정훈"
category: Project
tags: [Unity, Game, Project, Programming]
img : https://i.imgur.com/ZIiveoN.gif
comments_disable: true
---

# 멤버 소개 카드 게임 개발
## 역할 : 코드 전체 리팩토링 및 수정 디버깅

- 전체 팀원들의 코드를 취합하여 부족한 기능 구현에 대한 완성
- GameManger 작성
- 전체 UI 수정
- 카드 매치 로직 디버깅 
- 팀원 전체 코드 리뷰

# StartScene Script
## Class StartScene

#### 기존 코드

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class StartScene : MonoBehaviour
{
    public GameObject profile;
    public GameObject strBtn;
    public AudioSource audioSource;
    public AudioClip clip;
    private void Awake()
    {
        Button btn = strBtn.transform.GetComponent<Button>();
        btn.onClick.AddListener(startBtn);
        audioSource.clip = clip;
        audioSource.Play();
        for (int i = 0; i < 6; i++)
        {
            GameObject newProfile = Instantiate(profile);
            newProfile.transform.parent =
            GameObject.FindWithTag("LoopObjcet").transform;
            float x = (i % 7) * 3.5f - 7f;
            if (i % 2 == 0)
                newProfile.transform.position = new Vector3(x, 0.4f, 0);
            else
                newProfile.transform.position = new Vector3(x, -0.4f, 0);

            newProfile.transform.GetChild(0)
            .GetComponent<SpriteRenderer>().sprite 
            = Resources.Load<Sprite>(i.ToString());

        }
    }
    public void startBtn()
    {   
        SceneManager.LoadScene("Game");
    }
}
```

#### 문제점

>  1. btn OnClick 이벤트 리스너 실행 시 버튼 사운드 이펙트 미구현
>  2. 메소드 선언 시 컨벤션 문제 `public void startBtn()`

#### 수정 코드

##### Awake();

```csharp
btn.onClick.AddListener(() =>
		{
			// by 정훈
			// Start 버튼 클릭시 사운드 재생을 위해 코루틴 호출
			StartCoroutine(WaitForTime());
		});
```

##### WaitForTime();

```csharp
private IEnumerator WaitForTime()
{
	// by 정훈
	// 사운드 재생
	strBtn.GetComponent<AudioSource>().Play();
	// 버튼 누르고 대기 시간 지정 0.5초
	yield return new WaitForSeconds(0.3f);
	StartBtn();
}
```

- AddListener 의 CallBack으로 `Game Scene 로드 전`에 코루틴을 이용하여 딜레이 설정
- 버튼을 누르면 0.3초 짜리 AudioClip이 실행되고 끝나는 시간에 `StartBtn()` 호출하여 씬 이동

##### startBtn() => StartBtn(); 으로 변경

```csharp
// by 정훈
// 메소드 선언은 첫 문자는 UpperCase
private static void StartBtn()
{
	SceneManager.LoadScene("Game");
}
```

## Class profile

#### 기존 코드

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class profile : MonoBehaviour
{
    [SerializeField]
    private float xSpeed;
    [SerializeField]
    private float ySpeed;

    void Update()
    {  
        if (transform.position.x < -10.5)
        {   
            transform.position = new Vector3(10.5f, transform.position.y, 0);
        }
    }
    private void FixedUpdate()
    {   
        if (transform.position.y >= 0.41f)
            ySpeed *= -1;
        else if (transform.position.y <= -0.41f)
            ySpeed *= -1;
        transform.position += new Vector3(xSpeed, ySpeed, 0);
    }
}
```

#### 문제점

> 1. Class 생성 시 Class의 이름은 UpperCase로 시작을 권장
> 2. 스크립트 중 Profile.cs 가 이미 존재하기 때문에 혼란이 발생 할 수 있음

#### 수정 코드

```csharp
// by 정훈
// profile class 는 이름이 겹쳐 혼동일 올 수 있기 때문에
// StartProfile로 변 
public class StartProfile : MonoBehaviour
{

}
```

# Start Scene View

![](https://i.imgur.com/ZIiveoN.gif)

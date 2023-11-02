---
title: Mini Game Refactoring
author: "이정훈"
category: Project
tags: [Unity, Game, Project, Programming]
img : https://i.imgur.com/ZIiveoN.gif
comments_disable: true
---

![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb260cae4-a3d0-448b-be5d-7486d5925148%2F34.png?table=block&id=9e7562fc-62db-4d05-bb21-4e95a2e04542&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

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

## Start Scene View

![](https://i.imgur.com/ZIiveoN.gif)


---

# GameScene Script

## 씬 구성

> Scene 구성
> GameManager
> CardManager
> Card
> MemberCheck
> Profile

## Class GameManager

```csharp
using System;
using System.Collections;
using TMPro;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.Serialization;

namespace Script._02.GameScene
{
    public class GameManager : MonoBehaviour
    {
        public static GameManager Instance;
        [SerializeField] private TextMeshProUGUI leftCountText;
        [SerializeField] private TextMeshProUGUI selectCountText;
        public int maxCount = 18;
        public int currentCount;
        public int selectScore;
        public int highScore;
        private const string ScoreKey = "Score";
        private const string CurrentScoreKey = "CurrentScore";
        
        private void Awake()
        {   
            //게임 난이도에 따라 maxCount 변경
            maxCount = PlayerPrefs.GetInt("Level");
            // GameManager SingleTon
            if (Instance == null)
            {
                Instance = this;
            }
            else
            {
                Destroy(gameObject);
            }
            
            // 현재 카운트를 최대 카운트로 초기화 
            currentCount = maxCount;
            leftCountText.text = $"남은 횟수 : {currentCount:00}";
            selectScore = 0;
            selectCountText.text = $"시도 횟수 : {selectScore:00}";
        }

        private void Start()
        {
            
            CardManager.Instance.ShuffleCard();
        }

        public void UpdateText()
        {
            leftCountText.text = $"남은 횟수 : {currentCount:00}";
            selectCountText.text = $"시도 횟수 : {selectScore:00}";
        }

        public IEnumerator End()
        {
            // 카운트가 남아 있으면 리턴
            if (currentCount > 0) yield break;
        
            // 화면 종료시 터치 스코어
            int highPoint = PlayerPrefs.GetInt(ScoreKey, highScore);
        
            if (selectScore > highPoint)
            {
                PlayerPrefs.SetInt(ScoreKey, selectScore);
            }
            else
            {
                PlayerPrefs.SetInt(CurrentScoreKey, selectScore);
            }
        
            yield return new WaitForSecondsRealtime(1.0f);
            // 카운트가 0 또는 0보다 작으면 EndScene 호출
            SceneManager.LoadScene("EndScene");
        }
    }
}
```

## Class CardManager

#### 기존 코드

```csharp
using Script._02.GameScene;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

public class CardManager : MonoBehaviour
{
    public GameObject card;
    public GameObject firstCard;
    public GameObject secondCard;

    public static CardManager I;

    void Awake()
    {
        I = this;
    }

    void Start()
    {
        int[] charactor = { 0, 0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5 };
        charactor = charactor.OrderBy(item => Random.Range(-1.0f,
        1.0f)).ToArray();

        for (int i = 0; i < 12; i++)
        {
            GameObject newCard = Instantiate(card);
            newCard.transform.parent = GameObject.Find("CardManager").transform;

            float x = (i % 3) * -1.6f + 7.7f;
            float y = (i % 4) * -2.2f + 2.7f;
            newCard.transform.position = new Vector2(x, y);

            string charName = "charactor" + charactor[i].ToString();

			newCard.transform.Find("Front")
			.transform.GetChild(0).GetComponent<SpriteRenderer>().sprite = 
			Resources.Load<Sprite>(charName);
        }
    }

    public void isMatched()
    {
        string firstCardImage = 
        firstCard.transform.Find("Front").GetComponent<SpriteRenderer>()
        .sprite.name;
        string secondCardImage = 
        secondCard.transform.Find("Front").GetComponent<SpriteRenderer>()
        .sprite.name;

        if (firstCardImage == secondCardImage)
        {
            firstCard.GetComponent<Card>().destroyCard();
            secondCard.GetComponent<Card>().destroyCard();
        }
        else
        {
            firstCard.GetComponent<Card>().closeCard();
            secondCard.GetComponent<Card>().closeCard();
        }
        
        firstCard = null;
        secondCard = null;
    }
}
```

#### 문제점

> 1. `GameObject card` 변수가 Prefab 인지 그냥 오브젝트인지 구분이 잘 되지 않는 모호한 변수 선언
> 2. card 라는 Calss가 이미 존재
> 3. SingleTon 선언에 추가적인 인스턴스 생성을 방지하는 안전장치 부재
> 4. Find() 를 이용한 실시간 오브젝트 검색으로 발생하는 리소스 낭비
> 5. LifeCycle을 고려하지 않은 함수 사용방법
> 6. 함수 선언시 UpperCase 스타일 미적용
> 7. 문자열 합 형식의 오타 확률 
> 8. Card Class에 대한 상호 의존성의 문제  (기능 구분의 모호함)
> 9. Card 매치 관련된 기능 Bug 발생 및  기능 미구현
#### 수정 코드

##### 1,2. 변수명 변경
- 구분이 확실하거나 애매한 부분의 변경

```csharp
/*
* by 정훈
* 좀더 알기 쉬운 변수명으로 변경
* Card라는 클래스가 있으므로 타입은 GameObject가 아닌 Card로 (객체지향)
* card => cardPrefabs
*/
public Card cardPrefab;
public Card FirstSelectCard { get; set; }
public Card SecondSelectCard { get; set; }

private readonly List<Card> _cardList = new List<Card>();

private AudioSource _shuffleSound;

public int cardFlipCount;

/*
 * card Instance에서 접근하기 위한 SingleTon 생성
 * first, second Card에 접근
 */
public static CardManager Instance;

public bool isShuffling;

[SerializeField] private MemberCheck memberCheck;
```

##### 3,4. SingleTon, ObjectPooling 최적화
- Scene이동에서 발생할 수 있는 추가 적인 인스턴스 생성을 막기 위한 안전장치 설정
- Find() 메소드로 불필요한 Overhead 발생을 Pooling 방식 및 직렬화하여 최적화

```csharp
private void Awake()
{
	if (Instance == null)
	{
		Instance = this;
	}
	else
	{
		Destroy(gameObject);
	}

	_shuffleSound = GetComponent<AudioSource>();

	/*
	* by 정훈
	* 카드 생성시 발생하는 딜레이를 줄이기 위해
	* ObjetPooling 방식 사용
	*/

	int[] character = { 0, 0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5 };
	character = character.OrderBy(item => Random.Range(-1.0f, 1.0f)).ToArray();
	for (int i = 0; i < 12; i++)
	{
		/*
		* by 정훈
		* Prefabs를 인스턴스화 할때 Instantiate (origin GameObject, Transform 
		* transform, StayPosition bool) 속성을 사용하여 불필요한 transform 액세스 방지
		* 직렬화를 통한 Find 메소드 최적화
		*/
		Card newCard = Instantiate(cardPrefab, transform, true);
		newCard.transform.position = transform.position;
		newCard.character.name = character[i].ToString();
		newCard.character.GetComponent<SpriteRenderer>().sprite = 
		Resources.Load<Sprite>(newCard.character.name);
		newCard.character.SetActive(false);
		newCard.gameObject.SetActive(false);
		_cardList.Add(newCard);
	}
}
```

##### 5. LIfeCycle 
- 개발자의 의도에 맞는 실행 순서를 지켜 의도하지 않은 실행 및 움직임데 대한 통제 권한 생성

```csharp
public void ShuffleCard()
{
	isShuffling = true;
	int cardsMoved = 0;
	
	/*
	* by 정훈
	* 카드 생성시 한 지점에서 목표 지점으로 카드가 날아가는 움직임 구현
	*/
	float delay = 0.2f; // 각 카드 사이의 지연 시간
	float duration = 1.5f; // 카드가 이동하는 데 걸리는 시간
	_shuffleSound.Play();
	for (int i = 0; i < _cardList.Count; i++)
	{
		Card card = _cardList[i];
		float x = (i % 4) * -1.6f + 7.7f;
		float y = (i % 3) * -2.2f + 2.7f -2f;
		Vector2 targetPosition = new Vector2(x, y);
		
		DOVirtual.DelayedCall(i * delay, () =>
		{
			card.gameObject.SetActive(true);
			card.transform.DOMove(targetPosition, duration).OnComplete(() =>
			{
				// 이동을 완료한 카드 수를 증가시킵니다.
				cardsMoved++; 
				
				// 모든 카드가 이동을 완료했는지 확인합니다.
				if (cardsMoved == _cardList.Count)
				{
					// 모든 카드의 이동이 완료되면 isShuffling을 false로 설정합니다.
					isShuffling = false;
				}
			});
		});
	}
}
```

##### 8,9. 코드 의존성 및 최적화

```csharp
/*
* by 정훈
* 함수 선언 규칙은 UpperCase
* 해당 매치에 대한 판단 로직은 CardManager에서 해야 하지만
* 매치 메소드의 호출을 선택이 되는 Card Class 에서 진행 되어야 함
*/
public void IsMatched()
{
// string firstCardImage = firstSelectCard.transform.Find("Front").transform.GetChild(0).GetComponent<SpriteRenderer>().sprite.name;
// string secondCardImage = secondSelectCard.transform.Find("Front").transform.GetChild(0).GetComponent<SpriteRenderer>().sprite.name;

	if (FirstSelectCard.character.name == SecondSelectCard.character.name)
	{
		/*
		* by 정훈
		* Destroy의 경우 호출시 막대한 리소스를 소모하기 때문에
		* 해당 카드의 오브젝트를 비활성화 하는 것으로 변경
		* 매치된 멤버의 경우 MemberCheck 클래스를 호출하여 매치 리스트 최신화
		*/
		FirstSelectCard.gameObject.SetActive(false);
		SecondSelectCard.gameObject.SetActive(false);
		memberCheck.MatchMember(FirstSelectCard.character.name);
	}
	else
	{
		FirstSelectCard.CloseCard();
		SecondSelectCard.CloseCard();
	}
}
```


---

## Card Class

#### 기존 코드
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using System.Linq;
using Script._02.GameScene;
using DG.Tweening.Core.Easing;

public class Card : MonoBehaviour
{
    public Animator anim;

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Vector2 rayPos = Camera.main.ScreenToWorldPoint(Input.mousePosition);
            RaycastHit2D hit = Physics2D.Raycast(rayPos, Vector2.zero, 0f);

            if (hit.transform.gameObject == gameObject)
            {
                Debug.Log("You clicked on: " + hit.transform.name); 
                OpenCard();
            }
        }
    }


    public void OpenCard()
    {
        anim.SetBool("CardOpen", true);
        transform.Find("Front").gameObject.SetActive(true);
        transform.Find("Back").gameObject.SetActive(false);

        if (CardManager.I.firstCard == null)
        {
            CardManager.I.firstCard = gameObject;
        }
        else
        {
            CardManager.I.secondCard = gameObject;
            CardManager.I.isMatched();
        }
    }
    
    public void destroyCard()
    {
        Invoke("destroyCardInvoke", 1.0f);
    }

    void destroyCardInvoke()
    {
        Destroy(gameObject);
    }

    public void closeCard()
    {
        Invoke("closeCardInvoke", 1.0f);
    }

    void closeCardInvoke()
    {
        anim.SetBool("CardOpen", false);
        transform.Find("Back").gameObject.SetActive(true);
        transform.Find("Front").gameObject.SetActive(false);
    }
}
```

#### 문제점

> 1. 카드 더블클릭가능 및 플릭 Bug 발생
> 2. CardManager와의 의존성 문제
> 3. Prefab으로 설정되어 있는 오브젝트를 SingleTon으로 추가 인스턴스를 생성
> 4. 매번 매칭 시 Destroy 함수 사용으로 불필요한 리소스 낭비
> 5. 함수 선언시 UpperCase 양식 미적용

#### 수정코드

##### 1. Bug 발생 예외처리
- bool 값은 이용한 Bouncing
- 선택 카드의 할당값을 확인하여 예외처리

```csharp
 void Update()
	{
		// 카운트 '0'되면 즉시 게임 종료
		if (GameManager.Instance.currentCount <= 0)
		{
			StartCoroutine(GameManager.Instance.End());
		}

		if (Input.GetMouseButtonDown(0)) // left mouse button
		{
			if (Time.time - _lastClick < MinClickSpeed)
			{
				return;
			}

			_lastClick = Time.time;

			if (CardManager.Instance.cardFlipCount >=2) return;
			Vector2 rayPos =
			Camera.main.ScreenToWorldPoint(Input.mousePosition);
			RaycastHit2D hit = Physics2D.Raycast(rayPos, Vector2.zero, 0f);

			// if(hit.transform != null)
			// {
			//     Debug.Log("Hit Object: " + hit.transform.name); 
			//     Log the name of the object hit by the raycast
			//     Debug.Log($"Count:{CardManager.Instance.cardFlipCount}");
			// }
			// else
			// {
			//     Debug.Log("No hit");
			//     Debug.Log($"Count:{CardManager.Instance.cardFlipCount}");
			// }

			/*
			 * by 정훈
			 * 같은 카드를 두변 연속으로 클릭하면 같은 카드로 인식해 매치가 진행되는 버그 해결
			 */
			
			if (CardManager.Instance.SecondSelectCard == null)
			{
				Debug.Log("1");
				if (hit.transform != null && gameObject != null)
				{    
					Debug.Log("2");
					if (hit.transform.gameObject == gameObject)
					{
						Debug.Log("3");
						// 첫 번째 선택한 카드와 두 번째 선택한 카드가 같은지 확인
						if (CardManager.Instance.FirstSelectCard != null && 
							CardManager.Instance.FirstSelectCard == this)
						{
							Debug.Log("4");
							// 같은 카드를 두 번 클릭했으므로 무시
							return;
						}
						// 카드를 열고 처리
						OpenCard(this);
					}
				}
			}
		}
	}
```

##### 2, 3. 버그 발생 bool 값을 이용한 Bouncing, 불필요한 인스턴스 생성
- SigleTon 삭제
- 애니메이션에서 발생하던 버그를 DOTWeen으로 바꾸어 버그 방지

```csharp
private void OpenCard(Card selectCard)
{
	if (CardManager.Instance.isShuffling) return;
	if (_isFliping)return;
	
	_isFliping = true;
	
	Debug.Log("OpenCard");
	CardManager.Instance.cardFlipCount++;
	/*
	* by 정훈
	* 현재 카운트가 0 이하이면 EndScene으로 이동
	*/
	
	// anim.SetBool("CardOpen", true);
	
	/*
	* by 정훈
	* 스크립트에서 해당 오브젝트를 미리 할당하여 Find 메소드로 인한 리소스 소모 최적화
	* DOTween을 이용한 Card Flip 효과 구현
	*/
	
	// 카드 초기화
	card.GetComponent<SpriteRenderer>().sprite = backSprite;
	
	// 카드 Flip
	_flipSound.Play();
	float duration = 0.7f;
	transform.DORotate(new Vector3(0, 180, 0), duration)
		.SetEase(Ease.Linear)
		.OnUpdate(() =>
		{
			if (transform.rotation.eulerAngles.y >= 90)
			{
				card.GetComponent<SpriteRenderer>().sprite = frontSprite;
				character.SetActive(true);
				}
			}).OnComplete(() =>
			{
				if (CardManager.Instance.FirstSelectCard == null)
				{
					CardManager.Instance.FirstSelectCard = selectCard;
				}
				else if (CardManager.Instance.FirstSelectCard != null 
				&& CardManager.Instance.SecondSelectCard == null)
				{
					CardManager.Instance.SecondSelectCard = selectCard;
					GameManager.Instance.selectScore++;
					GameManager.Instance.currentCount--;
					GameManager.Instance.UpdateText();
					StartCoroutine(DelayedIsMatched());
					
					//if (GameManager.Instance.currentCount <= 0)
					//{
					//    StartCoroutine(GameManager.Instance.End());
					//}
					//Updata 부분으로 이동 by 정선교
						
			}
			_isFliping = false;
		});
}
```

##### 4. Destroy 함수 및 코드 컨벤션
- 의존성을 분리하기 위해 CardManager Class로 카드에 대한 권한 변경
- 불필요한 Card 오브젝트로 인해 발생하는 버그를 방지하고 코드의 재사용성을 확보

```csharp
private IEnumerator DelayedIsMatched()
{
	/*
	* by 정훈
	* 매치 확인 시 매치가 되면 카드가 바로 없어지는 것 처럼 보여 0.3초간의 텀을 주고 확인
	*/
	yield return new WaitForSecondsRealtime(0.3f);
	CardManager.Instance.IsMatched();
}

// public void destroyCard()
// {
//     Invoke("destroyCardInvoke", 1.0f);
// }
//
// void destroyCardInvoke()
// {
//     Destroy(gameObject);
// }

/*
* by 정훈 
* 함수 선언 규칙 UpperCase
*/
public void CloseCard()
{
	Invoke(nameof(CloseCardInvoke), 1.0f);
}

private void CloseCardInvoke()
{
	// anim.SetBool("CardOpen", false);
	/*
	* by 정훈
	* Find 메소드 최적화
	*/
	float duration = 0.3f;
	_flipSound.Play();
	transform.DORotate(new Vector3(0, 0, 0), duration).SetEase(Ease.Linear)
	.OnUpdate(() =>
	{
		if (transform.rotation.eulerAngles.y <= 90)
		{
			card.GetComponent<SpriteRenderer>().sprite = backSprite;
			character.SetActive(false);
		}
	}).OnComplete(() =>
	{
		CardManager.Instance.FirstSelectCard = null;
		CardManager.Instance.SecondSelectCard = null;
		CardManager.Instance.cardFlipCount--;
		Debug.Log($"Count : {CardManager.Instance.cardFlipCount}");
	});
	
	// transform.Find("Back").gameObject.SetActive(true);
	// transform.Find("Front").gameObject.SetActive(false);
	// backCard.SetActive(true);
	// frontCard.SetActive(false);
	// _cardFlipingCount = 0;
}
```


---

## Class MemberCheck 

#### 기존 코드

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MemberCheck : MonoBehaviour
{
    public GameObject[] namePanel = new GameObject[6];

    // ���� ī�尡 ��������, �̸��� ǥ��
    public void CharactorActive(string name)
    {
        // nameCheck0 : ������
        // nameCheck1 : ������
        // nameCheck2 : ���ǿ�
        // nameCheck3 : �����
        // nameCheck4 : ������
        // nameCheck5 : ������
        switch (name)
        {
            case "charactor0": // ������
                namePanel[0].gameObject.GetComponent<SpriteRenderer>().color 
                = new Color(0, 0, 0, 200 / 255f);
                break;

            case "charactor1": // �����
                namePanel[3].gameObject.GetComponent<SpriteRenderer>().color 
                = new Color(0, 0, 0, 200 / 255f);
                break;

            case "charactor2": // ������
                namePanel[1].gameObject.GetComponent<SpriteRenderer>().color 
                = new Color(0, 0, 0, 200 / 255f);
                break;

            case "charactor3": // ���ǿ�
                namePanel[2].gameObject.GetComponent<SpriteRenderer>().color 
                = new Color(0, 0, 0, 200 / 255f);
                break;

            case "charactor4": // ������
                namePanel[4].gameObject.GetComponent<SpriteRenderer>().color 
                = new Color(0, 0, 0, 200 / 255f);
                break;

            case "charactor5": // ������
                namePanel[5].gameObject.GetComponent<SpriteRenderer>().color 
                = new Color(0, 0, 0, 200 / 255f);
                break;

            default:
                break;
        }
    }
}
```

#### 문제점

> 1. 반복되는 코드에 대한 재사용성이 부족
> 2. case의 경우 비교값은 string으로 하는 것은 bug의 발생을 높힐 수 있음

#### 수정 코드

##### 1. 반복 코드 재사용성 확보
- 계속해서 반복적으로 사용 되는 코드에 대한 함수 선언으로 코드의 재사용성 확보

```csharp
public void MatchMember(string memberName)
{
	// nameCheck0 : 정선교
	// nameCheck1 : 박정혁
	// nameCheck2 : 선건우
	// nameCheck3 : 나재민
	// nameCheck4 : 이정훈
	// nameCheck5 : 장지후
	/*
	* by 정훈
	* Code 재사용을 위한 함수 생성 
	*/
	int nameNumber = int.Parse(memberName);
	Checker(nameNumber);
	
	// 기존 코드
	// switch (name)
	// {
	//     case "charactor0": // ������
	//         namePanel[0].gameObject.GetComponent<SpriteRenderer>().color 
	// = new Color(0, 0, 0, 200 / 255f);
	//         break;
	//     case "charactor1": // �����
	//         namePanel[3].gameObject.GetComponent<SpriteRenderer>().color 
	// = new Color(0, 0, 0, 200 / 255f);
	//         break;
	//     case "charactor2": // ������
	//         namePanel[1].gameObject.GetComponent<SpriteRenderer>().color 
	// = new Color(0, 0, 0, 200 / 255f);
	//         break;
	//     case "charactor3": // ���ǿ�
	//         namePanel[2].gameObject.GetComponent<SpriteRenderer>().color 
	// = new Color(0, 0, 0, 200 / 255f);
	//         break;
	//     case "charactor4": // ������
	//         namePanel[4].gameObject.GetComponent<SpriteRenderer>().color 
	// = new Color(0, 0, 0, 200 / 255f);
	//         break;
	//     case "charactor5": // ������
	//         namePanel[5].gameObject.GetComponent<SpriteRenderer>().color 
	// = new Color(0, 0, 0, 200 / 255f);
	//         break;
	//     default:
	//         break;
	// }
}

private void Checker(int nameNumber)
{
	/*
	* by 정훈
	* 멤버 리스트 중 memberNumber가 동일한 오브젝트만 컬러 및 체커 활성화
	*/ 
	foreach (var member in memberList)
	{
		if (member.memberNumber == nameNumber)
		{
		member.checkerBox.GetComponent<Image>().color 
		= new Color(0.0392f, 1f, 0f, 1f);
		member.checker.gameObject.SetActive(true);
		profile.ProfileOpen(member.memberNumber);
		memberCount++; // 조건이 충족될 때마다 카운트를 증가시킴 by 선교
		// Debug.Log($"menbercount : {memberCount}");
		// Profile GameObject의 활성화 상태를 확인
		}
	}
}
```


---


## Class Profile 

#### 기존 코드

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;
using UnityEngine.UI;

public class Profile : MonoBehaviour
{
	public GameObject ProfileBox; // 프로파일 UI 지정
	public GameObject ProfileImg1; // 프로파일 UI의 상단 이미지
	public GameObject ProfileImg2; // 프로파일 UI의 하단 이미지
	public Text Name; // 프로파일 UI 이름, MBTI 출력란
	public Text ProfileText; // 프로파일 UI 내용 출력란
	public int CardTypematch;
	private List<TeamMember> teamMembers;
	
	// Create a class to hold each row of data
	public class TeamMember
	{
		public int Num;
		public string Name;
		public string Mbti;
		public string Txt;
	}
	
	void OnEnable()
	{
		// UI가 활성화될 때 호출됩니다.
		LoadDataAndRefreshUI();
	}
	
	public void LoadDataAndRefreshUI()
	{
		TextAsset teamData = Resources.Load<TextAsset>("Team4DB");
		
		string[] data = teamData.text.Split(new char[] { '\n' });
		teamMembers = new List<TeamMember>();
		
		for (int i = 1; i < data.Length; i++) // Skip the first row (header)
		{
			string[] row = data[i].Split(new char[] { ',' });
			
			TeamMember t = new TeamMember();
			t.Num = int.Parse(row[0]);
			t.Name = row[1];
			t.Mbti = row[2];
			t.Txt = row[3];
			
			teamMembers.Add(t);
		}
	
		UpdateUI();
	}
	
	public void UpdateUI()
	{
		// Output the loaded data
		foreach (TeamMember member in teamMembers)
		{
			// CardTypematch와 같은 번호의 데이터만 출력하도록 설정
			if(CardTypematch == member.Num) 
			{
				string name = "이름 : ";
				string mbtiType = "MBTI 유형 :";
				// 가져올 스프라이트 이름 설정
				string CardImgname1 = "ProfileImg/"+member.Name + "_S"; 
				// 가져올 스프라이트 이름 설정
				string CardImgname2 = "ProfileImg/"+member.Name;
				// CardImgname와 동일한 이름의 이미지를 가져오도록 설정
				Sprite loadedSprite = Resources.Load<Sprite>(CardImgname1);
				// CardImgname2와 동일한 이름의 이미지를 가져오도록 설정
				Sprite loadedSprite2 = Resources.Load<Sprite>(CardImgname2);
				
				ProfileImg1.GetComponent<Image>().sprite = loadedSprite;
				ProfileImg2.GetComponent<Image>().sprite = loadedSprite2;
				// 이름 MBTI 이름 설정 지정
				Name.text = name + member.Name + '\n' + mbtiType + member.Mbti; 
				// 텍스트 설정 지정
				ProfileText.text = member.Txt;
				Debug.Log(CardImgname2);
				Debug.Log(
				$"Num: {member.Num}, Name: {member.Name}, Mbti: {member.Mbti}, Txt: {member.Txt}");
			}
		}
	}
	
	public void ProfileOpen()
	{
		ProfileBox.SetActive(true); // 닫기 버튼 클릭시 프로필 UI 닫기
	}
	
	public void Close()
	{
		ProfileBox.SetActive(false); // 닫기 버튼 클릭시 프로필 UI 닫기
	}
	
	private void Start()
	{
		ProfileBox.SetActive(false);
	}
}
```

#### 문제점

> 1. 데이터를 매 함수 호출마다 반복적으로 접근해서 불필요한 리소스 낭비
> 2. 이벤트 함수를 사용하지 않고 Inpector에서 직접 OnClick을 사용해서 이후 추가 기능에 대한 문제 발생

#### 수정 코드

##### 1. 데이터 최적화
- 스크립트 실행시 미리 데이터를 저장하고, 매개변수를 받아 해당 데이터만 Load

```csharp
/*
* by 정훈
* 패널이 열릴때 데이터를 읽어 오는 것이 아닌
* 미리 데이터를 변수에 저장하여 해당 멤버의 데이터만 추후 패널이 활성화 되면 가지고 오는 방식이
* 좀 더 효과적
*/

private void Awake()
{
	LoadDataAndRefreshUI();
	closeBtn.onClick.AddListener(Close);
}
// void OnEnable()
// {
//     // UI가 활성화될 때 호출됩니다.
//     LoadDataAndRefreshUI();
// }

public void LoadDataAndRefreshUI()
{
	TextAsset teamData = Resources.Load<TextAsset>("Team4DB");
	
	string[] data = teamData.text.Split(new char[] { '\n' });
	teamMembers = new List<TeamMember>();
	
	for (int i = 1; i < data.Length; i++) // Skip the first row (header)
	{
		string[] row = data[i].Split(new char[] { ',' });
		
		TeamMember t = new TeamMember();
		t.Num = int.Parse(row[0]);
		t.Name = row[1];
		t.Mbti = row[2];
		t.Txt = row[3];
		
		teamMembers.Add(t);
	}

	// UpdateUI();
}

/*
* by 정훈
* MemberCheck Class로 부터 매개변수룰 받아 해당하는 인원의 데이터를 불러오가
*/
private void UpdateUI(int memberIndex)
{
	// Output the loaded data
	
	foreach (TeamMember member in teamMembers)
	{
		// 매개변수로 받은 Member Index와 비교하여 데이터 비교   
		if(memberIndex == member.Num) 
		{
			string name = "이름 : ";
			string mbtiType = "MBTI 유형 :";
			// 가져올 스프라이트 이름 설정
			string CardImgname1 = "ProfileImg/"+member.Name + "_S"; 
			// 가져올 스프라이트 이름 설정
			string CardImgname2 = "ProfileImg/"+member.Name; 
			// CardImgname와 동일한 이름의 이미지를 가져오도록 설정
			Sprite loadedSprite = Resources.Load<Sprite>(CardImgname1);
			// CardImgname2와 동일한 이름의 이미지를 가져오도록 설정
			Sprite loadedSprite2 = Resources.Load<Sprite>(CardImgname2);
			ProfileImg1.GetComponent<Image>().sprite = loadedSprite;
			ProfileImg2.GetComponent<Image>().sprite = loadedSprite2;
			// 이름 MBTI 이름 설정 지정
			Name.text = name + member.Name + '\n' + mbtiType + member.Mbti; 
			// 텍스트 설정 지정
			ProfileText.text = member.Txt; 
			// Debug.Log(CardImgname2);
			// Debug.Log($"Num: {member.Num}, Name: {member.Name}, Mbti: {member.Mbti}, Txt: { member.Txt}");
		}
	}
}
```

##### 2. AddListener
- AddListner 이벤트를 호출하여 CallBack으로 필요한 데이터를 조작할 수 있도록 수정

```csharp
public Button closeBtn;

private void Awake()
{
	LoadDataAndRefreshUI();
	closeBtn.onClick.AddListener(Close);
}

public void Close()
{
	CardManager.Instance.cardFlipCount = 0;
	Debug.Log($"Count : {CardManager.Instance.cardFlipCount}");
	CardManager.Instance.FirstSelectCard = null;
	CardManager.Instance.SecondSelectCard = null;
	ProfileBox.SetActive(false); // 닫기 버튼 클릭시 프로필 UI 닫기
}
```


---


# EndScene Script

#### 기존코드

```csharp
using DG.Tweening;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
using static UnityEngine.GraphicsBuffer;

public class EndScenejj : MonoBehaviour
{
    public GameObject memberCharacter;
    public Button restartBtn;
    public Button exitBtn;

    private void Awake()
    {
        restartBtn.onClick.AddListener(Restart);
        exitBtn.onClick.AddListener(Exit);
    }

    // Start is called before the first frame update
    void Start()
    {
        // memberCharacter.transform.DOMoveY(1, 1).SetLoops(-1, LoopType.Restart);
        Sequence moveSequence = DOTween.Sequence();
        moveSequence.Append(memberCharacter.transform.DOMoveY(1, 0.3f)); // Y ��ǥ�� 1�� �̵�
        moveSequence.Append(memberCharacter.transform.DOMoveY(0, 0.5f)); // Y ��ǥ�� 0���� �̵�
        moveSequence.AppendInterval(0.5f);
        moveSequence.SetLoops(-1, LoopType.Restart); // ���� ����
    }

    // Update is called once per frame
    void Update()
    {
        
    }
    public void Restart()
    {
        SceneManager.LoadScene("Game");
    }

    public void Exit()
    {
        SceneManager.LoadScene("Start");
    }
}
```

#### 추가 기능

> 1. 게임 종료 시 Title Effect 및 Animation 추가

```csharp
private void Start()
{
	/*
	* by 정훈
	* 조 이름 회전 및 컬러 변경
	*/
	StartCoroutine(MoveInCircle());
	DOTween.To(() => _creditText.color, x => _creditText.color = x, new Color(Random.value, Random.value, Random.value, 1), colorChangeDuration)
	.SetLoops(-1, LoopType.Yoyo)
	.OnStepComplete(() =>
	{
		DOTween.To(() => _creditText.color, x => _creditText.color = x, 
		new Color(Random.value, Random.value, Random.value, 1), 
		colorChangeDuration);
	});
}

private IEnumerator MoveInCircle()
{
	Vector3 originalPosition = circleCenter;
	float timer = 0.0f;
	float circleDuration = 5.0f;

	while (true)
	{
		float angle = timer / circleDuration * 360 * Mathf.Deg2Rad;
		Vector3 offset = new Vector3(Mathf.Cos(angle) * radius, 
									 Mathf.Sin(angle) * radius, 0);
		endingCredit.transform.position = originalPosition + offset;
		
		timer += Time.deltaTime;
		if (timer > circleDuration) timer -= circleDuration;
		
		yield return null;
	}
}
```


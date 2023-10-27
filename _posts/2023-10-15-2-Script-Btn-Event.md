---
title: Unity Script Event Processing
category: Unity
author: 이정훈
tags:
  - unity
img: https://i.imgur.com/l4Uyvpe.jpg
comments_disable: true
meta_description: Unity Script Event Processing
---
# 스크립트에서 Btn Event 처리하기
- RunTime에서 발생하는 이벤트 (Call back)처리
- UnityEvent 또는 Delegate 이용

```csharp
private void Start()  
{  
	// Unity Action Event 연결  
	_action = () => OnButtonClick(startBtn.name);  
	startBtn.onClick.AddListener(_action);  
	  
	// 무명 메서드 활용 이벤트 연결 방식  
	optionBtn.onClick.AddListener(delegate { OnButtonClick(optionBtn.name);});  
	  
	// 람다식 이벤트 연결 방식  
	shopBtn.onClick.AddListener(()=> OnButtonClick(shopBtn.name));  
}  
  
private void OnButtonClick(string msg)  
{  
	Debug.Log($"Click Btn Name : {msg}");  
}
```

## Lamda Expression (람다식)
```csharp
delegate value = (params1, params2, ...) => expression;
delegate value = (params1, params2, ...) => {function1; function2; ...};
delegate value = () => expression;
delegate value = () => {function1; function2; ...};
```

```csharp
매개변수가 없는 Lamda
() => Debug.Log("Hello World!");

하나의 매개변수를 가진 Lamda
x => x * x;

여러개의 매개변수를 가진 Lamda
(x, y) => x + y;

문장블록을 가진 Lamda
(x,y) => {
	int sum = x + y;
	return sum * sum;
}
```

- `LINQ 쿼리`, `List<T>.ForEach` 또는 `Enumerable.Select` 와 같은 고차 메서드를 제공시 사용

```csharp
List<int> numbers = new List<int> {1,2,3,4,5};

// Select를 이용한 Lamda로 각 수자를 제곱
var squared = numbers.Select(x => x * x);

// Where를 이용한 Lamda로 홀수를 필터링
var evenNumbers = numbers.Where(x => x % 2 == 0);
```
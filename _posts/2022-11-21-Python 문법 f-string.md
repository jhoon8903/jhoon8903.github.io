---
layout: post
title: Python f-string
subtitle: 파이썬의 문자열 보간
categories: Python
author: Daniel
tags:
 - Python
 - Programming
 - Language
banner:
 image : https://blog.kakaocdn.net/dn/bL8ETY/btrsc1dKdvU/PKl3b1kLTKsKuWAY9u2XT1/img.png
---

f-string
--

-   변수로 더 직관적인 문자열 만들기

```python
scores = [{'name':'영수', 'score':70},
		  {'name':'영희', 'score':65},
          {'name':'기찬', 'score':75},
          {'name':'희수', 'score':23},
          {'name':'서경', 'score':99},
          {'name':'미주', 'score':100},
          {'name':'병태', 'score':32}
          ]
 for s in scores:
 	name = s['name']
    score = str(s['score']) 					# 정수 > 문자로, 반대는 int()
    print(name+'의 점수는 '+score+'점 입니다.') 
    
    print(f'{name}의 점수는 {score}입니다.)  		# f-string
    
    >
    
    영수의 점수는 70점 입니다.
	영희의 점수는 65점 입니다.
	기찬의 점수는 75점 입니다.
	희수의 점수는 23점 입니다.
	서경의 점수는 99점 입니다.
	미주의 점수는 100점 입니다.
	병태의 점수는 32점 입니다.
```
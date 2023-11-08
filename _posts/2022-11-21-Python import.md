---
layout : post
title: Python import
subtitle: Import
categories: Python
author: Daniel
tags: 
 - Python
 - Programming
 - Language
banner:
 image : https://blog.kakaocdn.net/dn/bL8ETY/btrsc1dKdvU/PKl3b1kLTKsKuWAY9u2XT1/img.png
---

파일불러오기 'import'
--

## 여러개 파일로 분리하기

### app.py

```python
from app_func import * say_hi_to		#say_hi_to 만 불러온다
```

### app_func.py

```python
def say_hi():
	print('안녕!')
    
def say_hi_to(name):
	print(f'{name}님 안녕하세요')
```

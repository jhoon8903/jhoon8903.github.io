---
layout: post
title: Python Basic 1
subtitle : Python 변수, 자료형에 대해서
author: Daniel
categories: Python
tags: 
 - Python
 - Language
 - Programming
banner:
 image : https://blog.kakaocdn.net/dn/bL8ETY/btrsc1dKdvU/PKl3b1kLTKsKuWAY9u2XT1/img.png
---

Python 기초 문법
--
## 변수 & 기본연산

```null
a = 3			#3 을 a에 넣는다.
b = a 			#a를 b에 넣는다.
a = a+1 		#a+1을 다시 a에 넣는다.

num1 = a*b 		#a*b의 값을 num1이라는 변수에 넣는다.
num2 = 99		#99의 값을 num2이라는 변수에 넣는다.
```

## 자료형

### Number, Text Form

```null
name = 'bob'	#변수에는 문자열이 들어갈 수도 있고,
num = 12		#숫자가 들어갈 수도 있고,
is_number = True 또는 False > "Boolean"형이 들어갈 수도 있다.

# List, Dictionary도 들어갈 수도 있다.
```

### List form (Javascript의 배열형과 동일)

```null
a_list = []
a_list.append(1)		# 리스트 값을 추가한다.
a_list.append([2,3]) 	# 리스트에 [2,3]이라는 리스트를 다시 넣는다.

##################################################################

a_list의 값은 [1,[2,3]]
a_list[0]의 값은 1
a_list[1]의 값은 [2,3]
a_list[1][0]의 값은 2
```

### Dictionary form (Javascript의 Dictionary과 동일)

```null
a_dict = {}
a_dict = {'name' : 'bob', 'age' : 21}
a_dict = ['height'] = 178

##############################################################

a_dict의 값은 {'name' : 'bob', 'age' : 21, 'height' : 178}
a_dict {'name'} 값은 'bob'
a_dict {'age'} 값은 '21'
a_dict {'height'} 값은 '178'
```

### Dictionary & List form combination

```null
people = [{'name' : 'bob', 'age' : 20},{'name' : 'carry', 'age' : 38}

people[0]['name']값은 'bob'
people[1]['name'] 값은 'carry'

person = {'name' : 'jhon', 'age' : 7}
people.append(person)

people의 값은 [{'name' : 'bob', 'age' : 20},{'name' : 'carry', 'age' : 38},{'name' : 'jhon', 'age' : 7}]
people[2]['name'] 값은 'jhon'
```